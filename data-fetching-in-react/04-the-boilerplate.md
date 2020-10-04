# Boilerplaten

## APIet

```tsx
// api.ts
export const getUser = async () => {
  const response = await fetch("/api/user");
  if (!response.ok) {
    throw Error("User not found");
  }
  return response.json() as User;
};
```

## Actions og action creators

```tsx
enum UserActionsEnum {
  REQUEST = "FETCH_USER_REQUEST",
  RESPONSE = "FETCH_USER_RESPONSE",
  ERROR = "FETCH_USER_ERROR",
}
type UserActions =
  | { type: UserActionsEnum.REQUEST }
  | { type: UserActionsEnum.RESPONSE; user: User }
  | { type: UserActionsEnum.ERROR; error: Error };

const fetchUserRequest = (): UserActions => ({ type: UserActions.REQUEST });
const fetchUserResponse = (user: User): UserActions => ({
  type: UserActions.RESPONSE,
  user,
});
const fetchUserError = (error: Error): UserActions => ({
  type: UserActions.ERROR,
  error,
});
```

## Thunken

```tsx
import * as api from "./api";

export const fetchUser = (dispatch) => async () => {
  dispatch(fetchUserRequest());
  try {
    const user = await api.getUser();
    dispatch(fetchUserResponse(user));
  } catch (e) {
    dispatch(fetchUserError(error));
  }
};
```

## Reduceren

```tsx
type UserState = {
  isLoading: boolean;
  error?: Error;
  data?: User;
};
export const userReducer = (state: UserState, action: UserActions) => {
  switch (action.type) {
    case UserActionsEnum.REQUEST: {
      return {
        ...state,
        isLoading: true,
        error: undefined,
      };
    }
    case UserActionsEnum.RESPONSE: {
      return {
        isLoading: false,
        error: undefined,
        data: action.data,
      };
    }
    case UserActionsEnum.ERROR: {
      return {
        isLoading: false,
        error: action.error,
        data: undefined,
      };
    }
    default:
      return state;
  }
};
```

## Selectoren

```tsx
import { createSelector } from "reselect";

const userSelector = createSelector((state: State) => state.user);
```

## Bruken

```tsx
type ProfilePageProps = {
  user: User | null;
  isLoadingUser: boolean;
  userError: Error | null;
};
const ProfilePage: React.FC<ProfilePageProps> = ({
  user,
  isLoadingUser,
  userError,
}) => {
  if (!user && isLoadingUser) {
    return <Spinner />;
  }

  if (userError) {
    return <p>Oops, something crashed</p>;
  }

  return <h1>Hi there {user.name}</h1>;
};
};
export default connect((state) => ({
  user: userSelector(state).data,
  isLoadingUser: userSelector(state).isLoading,
  userError: userSelector(state).error,
}))(ProfilePage);
```
