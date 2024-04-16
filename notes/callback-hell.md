# Callback Hell Example:

## Before (aka Callback Hell):

```javascript
// Simulated API call to get user data
function getUser(userId, callback) {
    setTimeout(() => {
        console.log("Fetching user...");
        callback(null, { userId: userId, name: 'John' });
    }, 1000);
}

// Simulated API call to get user preferences
function getUserPreferences(user, callback) {
    setTimeout(() => {
        console.log("Fetching preferences for " + user.name);
        callback(null, { userId: user.userId, preferences: { theme: 'dark' } });
    }, 1000);
}

// Simulated API call to get settings based on preferences
function getUserSettings(preferences, callback) {
    setTimeout(() => {
        console.log("Fetching settings for " + preferences.preferences.theme + " theme");
        callback(null, { themeSettings: { fontSize: '16px' } });
    }, 1000);
}

// Fetch user, then fetch preferences, then fetch settings
getUser(1, (error, user) => {
    if (error) {
        console.log('Error fetching user:', error);
        return;
    }

    getUserPreferences(user, (error, preferences) => {
        if (error) {
            console.log('Error fetching preferences:', error);
            return;
        }

        getUserSettings(preferences, (error, settings) => {
            if (error) {
                console.log('Error fetching settings:', error);
                return;
            }

            console.log('Got user:', user);
            console.log('Got preferences:', preferences);
            console.log('Got settings:', settings);
        });
    });
});
```

## After (fixed with Promises)

```javascript
// Convert getUser to return a Promise
function getUser(userId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Fetching user...");
            resolve({ userId: userId, name: 'John' });
        }, 1000);
    });
}

// Convert getUserPreferences to return a Promise
function getUserPreferences(user) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Fetching preferences for " + user.name);
            resolve({ userId: user.userId, preferences: { theme: 'dark' } });
        }, 1000);
    });
}

// Convert getUserSettings to return a Promise
function getUserSettings(preferences) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            console.log("Fetching settings for " + preferences.preferences.theme + " theme");
            resolve({ themeSettings: { fontSize: '16px' } });
        }, 1000);
    });
}

// Use the promises with chaining
getUser(1)
    .then(user => {
        console.log('Got user:', user);
        return getUserPreferences(user);
    })
    .then(preferences => {
        console.log('Got preferences:', preferences);
        return getUserSettings(preferences);
    })
    .then(settings => {
        console.log('Got settings:', settings);
    })
    .catch(error => {
        console.log('Error:', error);
    });

```
