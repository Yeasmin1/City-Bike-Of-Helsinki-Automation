✅ How Google Login Works

    Login starts when the user clicks the "Google login" button:

const login = useGoogleLogin({ onSuccess: ..., onError: ... });

On successful login, Google returns a token, and you store that in user:

onSuccess: (userProfileResponse) => setUser(userProfileResponse)

A useEffect runs:

    useEffect(() => {
      if (user) {
        axios.get(...userinfo...)
          .then((res) => {
            setLoginProfile(res.data);
            sessionStorage.setItem('userProfile', JSON.stringify(res.data));
            navigate('/');
          });
      }
    }, [user]);

    ✅ This:

        Fetches the user's Google profile

        Stores it in local state loginProfile via setLoginProfile

        Saves it in sessionStorage

        Redirects to the homepage

✅ How Logout Works

In Navigation.tsx:

const handleLogout = () => {
  googleLogout();                        // Revoke Google session
  setLoginProfile(null);                // Clear local login state
  window.sessionStorage.clear();        // Clear session storage
  navigate('/');
};

✅ This:

    Revokes the Google session

    Clears the user profile from state and sessionStorage

    Returns the user to the home page

❓ Why You Need useEffect in App.tsx

Here’s the critical part.
🔁 Problem Without It:

If a user logs in and refreshes the page, loginProfile (stored in useState) will be lost — because it's only in memory.
🧠 What useEffect in App.tsx Does:

This block in App.tsx:

useEffect(() => {
  setPageDataInfo(Jsondata);
  const isLoggedIn = sessionStorage.getItem('userProfile');
  if (isLoggedIn) {
    setLoginProfile(JSON.parse(isLoggedIn));
  }
}, []);

✅ This:

    Loads your app content

    Restores the logged-in user's profile from sessionStorage

    Sets it back into React state using setLoginProfile

🎯 This allows:

    Persistent login across page reloads

    Navigation bar (Navigation.tsx) to keep showing the logged-in user

    The app to know the user is still authenticated

✅ Summary
Action	Where It Happens	What It Does
Google Login	LoginForm.tsx	Auth + get profile + store in state and sessionStorage
Session Restore	App.tsx useEffect	Reads sessionStorage and restores loginProfile
Logout	Navigation.tsx	Revokes Google, clears state + sessionStorage
✅ TL;DR

    Google login works via token → fetch → store → navigate

    Logout clears everything including Google session

    App.tsx needs a useEffect to restore login on refresh, otherwise user state is lost