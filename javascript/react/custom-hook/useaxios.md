# useAxios

```jsx
import defaultAxios from "axios";
import { useEffect, useState } from "react";

const useAxios = (opts, axiosInstance = defaultAxios) => {
  const [state, setState] = useState({
    loading: true,
    error: null,
    data: null
  });

  const [trigger, setTrigger] = useState(0);

  if (!opts.url) {
    return;
  }

  const refetch = () => {
    setState({
      ...state,
      loading: true
    });
    setTrigger(Date.now());
  };

  useEffect(() => {
    axiosInstance(opts)
      .then((data) => {
        setState({
          ...state,
          loading: false,
          data
        });
      })
      .catch((error) => {
        setState({
          ...state,
          loading: false,
          error
        });
      });
  }, [trigger]);

  return { ...state, refetch };
};

export default useAxios;

```

{% embed url="https://codesandbox.io/s/useaxios-4si71?fontsize=14&hidenavigation=1&theme=dark" %}



