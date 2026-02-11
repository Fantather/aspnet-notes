`@testing-library/user-event` — это библиотека, которая симулирует события браузера так, как они происходят при реальном взаимодействии пользователя со страницей.

Если `fireEvent` (встроенный в `@testing-library/react`) просто программно вызывает событие DOM (например, `element.dispatchEvent(new MouseEvent('click'))`), то `user-event` воспроизводит **полный цикл взаимодействия**.

### Основные отличия от fireEvent
**Полнота эмуляции**
  fireEvent.click(element) вызывает только событие click.
  userEvent.click(element) последовательно вызывает цепочку событий: hover → mousedown → focus → mouseup → click
  
  Это важно, так как некоторые компоненты могут полагаться на состояние фокуса или наведение курсора.

**Обработка ввода**
  fireEvent.change(input, { target: { value: 'a' } }) просто меняет значение свойства value и вызывает событие change.

  userEvent.type(input, 'a') имитирует нажатие клавиш (keydown, keypress, keyup) для каждого символа, что активирует обработчики вроде onKeyDown и правильно изменяет состояние инпута (например, положение каретки).

**Асинхронность**
  В версии 14+ все методы user-event являются асинхронными. Это связано с тем, что некоторые действия в браузере (например, вставка текста или переход по Tab) требуют разрыва в Event Loop.


### Как использовать (Синтаксис v14)
Для корректной работы рекомендуется инициализировать "пользователя" через метод `setup()` перед выполнением действий

``` JavaScript
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import MyComponent from './MyComponent'

test('ввод текста и клик', async () => {
  // 1. Инициализация сессии пользователя
  const user = userEvent.setup()

  render(<MyComponent />)

  // 2. Взаимодействие (обязательно await)
  const input = screen.getByRole('textbox')
  await user.type(input, 'Hello World') // Вводит посимвольно

  const button = screen.getByRole('button')
  await user.click(button) // Полный цикл клика

  // 3. Проверки
  expect(input).toHaveValue('Hello World')
})
```

### Ключевые методы
**setup()**
  Создает инстанс user-event с конфигурацией. Рекомендуется вызывать в начале теста.

**click(element)**
  Эмулирует клик левой кнопкой мыши с учетом состояния disabled и фокуса.

**type(element, text)**
  Эмулирует набор текста на клавиатуре. Поддерживает служебные клавиши, например {enter}, {backspace}, {esc}.

**tab()**
  Эмулирует нажатие клавиши Tab, перемещая фокус между элементами согласно tabIndex и порядку в DOM.

**clear(element)**
  Очищает поле ввода (выделяет текст и нажимает delete).

