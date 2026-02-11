### Границы ошибок (Error Boundaries)
Передача обработки ошибки родительскому компоненту (Error Boundary) с помощью флага `throwOnError`.

```jsx
const { data } = useQuery({
  queryKey: ['todos'],
  queryFn: fetchTodos,
  // Ошибка будет выброшена, и React поднимет её до ближайшего ErrorBoundary
  throwOnError: true, 
});
```

### Глобальный Callbacks
Для централизованного показа уведомлений (toast) лучше использовать глобальный `QueryCache`, а не локальные колбэки

``` js
import { QueryCache, QueryClient } from '@tanstack/react-query';
import toast from 'react-hot-toast';

const queryClient = new QueryClient({
  queryCache: new QueryCache({
    onError: (error) => {
      toast.error(`Произошла ошибка: ${error.message}`);
    },
  }),
});
```


### React Query DevTools
Инструмент для отладки, позволяющий видеть состояние кэша, ключи запросов и их статус
Обязателен для использования в режиме разработки

Подключается в корневой компонент
``` js
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

const App = () => (
  <QueryClientProvider client={queryClient}>
    {/* Твое приложение */}
    <ReactQueryDevtools initialIsOpen={false} />
  </QueryClientProvider>
);
```