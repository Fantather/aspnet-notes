В этом разделе разобраны частые сценарии, встречающиеся при разработке (например, поиск или фильтрация)

### Зависимые запросы (Dependent Queries)
Запрос выполняется только после того, как станут доступны данные для его выполнения. Это реализуется через параметр `enabled`.
```jsx
import { useQuery } from '@tanstack/react-query';

const UserProjects = ({ userId }) => {
  // 1. Получаем пользователя
  const { data: user } = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
  });

  // 2. Получаем проекты, ТОЛЬКО если user.id уже загружен
  const { data: projects } = useQuery({
    queryKey: ['projects', userId],
    queryFn: () => fetchProjects(user.id),
    // Запрос активируется только при наличии user.id
    enabled: !!user?.id, 
  });
};
```

### Трансформация данных (Select)
Использование опции `select` позволяет подписать компонент только на _часть_ данных или изменить их структуру

```js
const { data: todoTitles } = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodos,
  // Компонент обновится, только если изменится список заголовков
  select: (data) => data.map(todo => todo.title),
});
```


### Поллинг (Интервальные запросы)
Автоматическое обновление данных через заданные промежутки времени

```js
useQuery({
  queryKey: ['notifications'],
  queryFn: fetchNotifications,
  // Обновлять каждые 3 секунды
  refetchInterval: 3000,
  // Продолжать, даже если вкладка браузера не активна
  refetchIntervalInBackground: true, 
});
```

### Пагинация с сохранением предыдущих данных
Для предотвращения "мигания" интерфейса при смене страницы используется `placeholderData` (в v5 заменил `keepPreviousData`)

```js
import { keepPreviousData, useQuery } from '@tanstack/react-query';
import { useState } from 'react';

const ProjectsList = () => {
  const [page, setPage] = useState(1);

  const { data } = useQuery({
    queryKey: ['projects', page],
    queryFn: () => fetchProjects(page),
    // Сохраняет данные предыдущей страницы до окончания загрузки новой
    placeholderData: keepPreviousData, 
  });
};
```