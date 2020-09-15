# Et eksempel med context

```tsx
import * as api from "./api";

const UserContext = React.createContext<User | null>(null);

export const UserProvider: React.FC = ({ children }) => {
  const [user, setUser] = React.useState<User | null>(null);
  const [error, setError] = React.useState<string | null>(null);
  const [isLoading, setLoading] = React.useState(false);

  React.useEffect(() => {
    const fetchUser = async () => {
      try {
        setLoading(true);
        setUser(await api.getUser());
        setError(null);
      } catch (error) {
        setUser(null);
        setError(error);
      } finally {
        setLoading(false);
      }
    };
    fetchUser();
  }, []);

  return (
    <UserContext.Provider
      value={{ user, setUser, error, setError, isLoading, setLoading }}
    >
      {children}
    </UserContext.Provider>
  );
};
export const useUser = () => {
  const userContext = React.useContext(UserContext);
  return { user, setUser, error, setError, isLoading, setLoading };
};
```

```tsx
const App = () => {
  return (
    <UserProvider>
      <Router>
        <Route path="/profile" component={ProfilePage} />
        <Route path="/post/:postId" component={PostPage} />
      </Router>
    </UserProvider>
  );
};
```

```tsx
const ProfilePage = () => {
  const { user, error, isLoading } = useUser();
  if (isLoading) {
    return <Spinner />;
  }
  if (error) {
    return <p>Something crashed</p>;
  }
  return <h1>Hi there {user.name}</h1>;
};
```
