### Query Key Factory
Избегай использования строковых литералов ('magic strings') для ключей в разных частях приложения

Вместо ручного написания ключей (массивов строк) в каждом компоненте, используй единый объект. Это исключает опечатки и упрощает управление кэшем.

```javascript
// keys.js
export const todoKeys = {
  all: ['todos'],
  lists: () => [...todoKeys.all, 'list'],
  list: (filters) => [...todoKeys.lists(), { filters }],
  details: () => [...todoKeys.all, 'detail'],
  detail: (id) => [...todoKeys.details(), id],
};

// Использование в компоненте
useQuery({ 
  queryKey: todoKeys.list('active'), 
  queryFn: fetchTodos 
});
```

### Custom Hooks (Инкапсуляция)
Никогда не используй `useQuery` напрямую в UI-компонентах. Оборачивай их в кастомные хуки. Это разделяет слой данных и слой отображения, а также позволяет переиспользовать логику (например, `select`)

```js
// useTodos.js
export const useTodos = (filter) => {
  return useQuery({
    queryKey: ['todos', filter],
    queryFn: () => fetchTodos(filter),
    select: (data) => data.filter(t => !t.deleted),
    staleTime: 1000 * 60,
  });
};
```


### Дефолтные настройки QueryClient
Глобальные настройки задаются при создании `QueryClient`
Это позволяет установить базовое поведение для всего приложения (например, глобальную обработку ошибок или дефолтный `staleTime`)

``` js
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      // Данные считаются свежими 1 минуту
      staleTime: 1000 * 60, 
      // Глобальная логика повторных запросов
      retry: (failureCount, error) => {
        if (error.status === 404) return false;
        return failureCount < 3;
      },
    },
  },
});
```


### Предзагрузка (Prefetching)
Используй `queryClient.prefetchQuery` в обработчиках событий (например, `onMouseEnter` на ссылке), чтобы данные оказались в кэше еще до перехода пользователя на страницу. Это создает ощущение мгновенной загрузки.

``` js
const handleMouseEnter = () => {
  queryClient.prefetchQuery({
    queryKey: ['todo', id],
    queryFn: () => fetchTodo(id),
  });
};
```