# useState-Model
this document shows the basic model of useState and how it works.

/* eslint-disable react-refresh/only-export-components */
import { useEffect } from "react";
import { createRoot } from "react-dom/client";

const state = [];
let callCount = -1;

function useState(initialValue) {
  const id = ++callCount;

  if (state[id]) return state[id];

  const setValue = (newValue) => {
    state[id][0] = newValue;
    render();
  };

  const tuple = [initialValue, setValue];
  state.push(tuple);
  return tuple;
}

export default function Counter() {
  const [count, setVal] = useState(0);
  const [error, setError] = useState(null);
  const handlePlus = () => setVal(count + 1);
  const handleMines = () => setVal(count - 1);

  useEffect(() => {
    if (count >= 3) {
      setError("can be more than 3");
    } else {
      setError(null);
    }
  }, [count]);

  console.log("state", state);

  return (
    <>
      <button onClick={handlePlus}>+</button>
      <button onClick={handleMines}>-</button>
      <p>Count is: {count}</p>
      <p>{error}</p>
    </>
  );
}

const root = createRoot(document.getElementById("root"));

function render() {
  callCount = -1;
  root.render(<Counter />);
}

render();
