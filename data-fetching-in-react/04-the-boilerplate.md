# Boilerplaten

## Action Creatorene

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

## APIet

```tsx
// api.ts
export const getUser = await () => {
  const response = await fetch('/api/user');
  if (!response.ok) {
    throw Error("User not found");
  }
  return response.json() as User;
}
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

## Reducern

```tsx
type UserState = {
  isLoading: boolean;
  error?: Error;
  data?: User;
};
export const userReducer = (state: UserState, action: UserActions) {
  if (action.type === UserActionsEnum.REQUEST) {
    return {
        ...state,
        isLoading: true,
        error: undefined,
      };
  }
  if (action.type === UserActionsEnum.RESPONSE) {
      return {
        isLoading: false,
        error: undefined,
        data: action.data,
      };
    }
  if (action.type === UserActionsEnum.ERROR) {
    return {
      isLoading: false,
      error: action.error,
      data: undefined,
    };
  }
}
```
