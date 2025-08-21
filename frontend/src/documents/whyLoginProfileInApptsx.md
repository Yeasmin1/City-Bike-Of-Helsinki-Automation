Redux Thunk:

Why I need to call setloginprofile in app.tsx?
-Bcz Redux state is only in- memory. It gets wiped on page reload.

Redux does not persist state by default
when refresh bro
❗Redux does not persist state by default.
When you refresh the browser:
    • The whole React app re-initializes
    • Redux store resets to initial state (loginProfile: null)
    • Any authenticated user info is lost
Even though you saved it earlier to sessionStorage in your slice reducer:
window.sessionStorage.setItem('userProfile', JSON.stringify(action.payload));
That’s just browser storage — Redux has no idea it’s there unless you explicitly read it back in.
✅ That’s Why You Use setLoginProfile in App.tsx
This effect:
useEffect(() => {
  const storedUser = sessionStorage.getItem('userProfile');
  if (storedUser) {
    dispatch(setLoginProfile(JSON.parse(storedUser)));
  }
}, []);

Does this:
    • Reads from sessionStorage
    • Writes it back into Redux
    • Makes your app "remember" that the user was logged in after refresh
🔁 Without this:
    • auth.loginProfile is null on page load
    • The navigation will think the user is not logged in
    • You’ll lose login state in all components relying on Redux
✅ Optional: Use Redux Persist (Advanced)
You could use redux-persist to automatically save and restore the Redux store across page reloads.
But you're already handling this manually with sessionStorage, which is fin✅ Optional: Use Redux Persist (Advanced)
You could use redux-persist to automatically save and restore the Redux store across page reloads.
But you're already handling this manually with sessionStorage, which is fine and simple.
e and simple.
