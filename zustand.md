🧠 1. What Zustand is (in plain terms)
Zustand is global state management for React—but:

no providers

no boilerplate reducers

no context hell

👉 Think of it as:

“a simple global store you can access anywhere”

⚡ 2. The simplest possible store
import { create } from "zustand";

type CounterStore = {
count: number;
increment: () => void;
};

export const useCounterStore = create<CounterStore>((set) => ({
count: 0,
increment: () => set((state) => ({ count: state.count + 1 })),
}));
✅ Use it in a component
const count = useCounterStore((state) => state.count);
const increment = useCounterStore((state) => state.increment);

return <button onClick={increment}>{count}</button>;
🔑 Core idea (important)
create() → defines the store

set() → updates state

get() → reads state (inside store)

useStore(selector) → subscribe to specific state

🚀 3. Avoid re-renders (very important)
❌ Bad:

const store = useCounterStore(); // re-renders on ANY change
✅ Good:

const count = useCounterStore((s) => s.count);
👉 Always use selectors

🧱 4. Real-world example (form flow like yours)
type RegisterStore = {
step: number;
next: () => void;
back: () => void;
};

export const useRegisterStore = create<RegisterStore>((set) => ({
step: 0,
next: () => set((s) => ({ step: s.step + 1 })),
back: () => set((s) => ({ step: s.step - 1 })),
}));
✅ Use in your flow
const step = useRegisterStore((s) => s.step);
const next = useRegisterStore((s) => s.next);
👉 Now you don’t need props drilling anymore.

🧩 5. Updating state
Basic
set({ count: 10 });
Using previous state
set((state) => ({ count: state.count + 1 }));
🧠 6. Using get() inside store
const useStore = create((set, get) => ({
count: 0,
double: () => {
const current = get().count;
set({ count: current \* 2 });
},
}));
🧠 7. Async actions (very common)
const useUserStore = create((set) => ({
user: null,
loading: false,

fetchUser: async () => {
set({ loading: true });

    const res = await fetch("/api/user");
    const data = await res.json();

    set({ user: data, loading: false });

},
}));
🧠 8. Splitting store (scaling)
Don’t dump everything in one store.

// authStore.ts
// registerStore.ts
// uiStore.ts
👉 Keep stores feature-based

🧠 9. Persist state (localStorage)
import { persist } from "zustand/middleware";

export const useStore = create(
persist(
(set) => ({
step: 0,
next: () => set((s) => ({ step: s.step + 1 })),
}),
{
name: "register-storage",
}
)
);
🧠 10. Devtools (debug like Redux)
import { devtools } from "zustand/middleware";

export const useStore = create(
devtools((set) => ({
count: 0,
increment: () => set((s) => ({ count: s.count + 1 })),
}))
);
🧠 11. Best practices (this is where most people fail)
✅ DO
use selectors

split stores by feature

keep logic inside store

name actions clearly (setUser, logout, etc.)

❌ DON’T
store everything globally

mutate state directly

return whole store in components

🧠 12. Advanced pattern (clean architecture)
type AuthStore = {
user: null | { name: string };
setUser: (user: any) => void;
logout: () => void;
};

export const useAuthStore = create<AuthStore>((set) => ({
user: null,
setUser: (user) => set({ user }),
logout: () => set({ user: null }),
}));
🔥 13. Your exact use case (multi-step form)
Instead of props:

const step = useRegisterStore((s) => s.step);
const next = useRegisterStore((s) => s.next);
Now every component can do:

next();
👉 No prop drilling
👉 Cleaner architecture

⚡ 14. Mental model (important)
Think of Zustand as:

“a global React state with local simplicity”
