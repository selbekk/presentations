# Et eksempel med SWR

```tsx
import React from "react";
import useSWR from "swr";
import * as api from "./api";

const ProfilePage = () => {
  const { data: user, isValidating, error } = useSWR("user", api.getUser);

  if (!user && isValidating) {
    return <Spinner />;
  }

  if (error) {
    return <p>Oops, something crashed</p>;
  }

  return <h1>Hi there {user.name}</h1>;
};

const FanPage = () => {
  const { data: user, isValidating, error } = useSWR("user", api.getUser);

  if (!user && isValidating) {
    return <Spinner />;
  }

  if (error) {
    return <p>Oops, something crashed</p>;
  }

  return <h1>{user.name} is your biggest fan</h1>;
};
```
