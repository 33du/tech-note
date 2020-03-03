https://stackoverflow.com/questions/35411423/how-to-dispatch-a-redux-action-with-a-timeout/35415559#35415559

# Without Thunk
## Dispatch Action
Dispatch plain action object: `store.dispatch({ type: 'SHOW_NOTIFICATION'})` 

## Dispatch Action Creator
An **action creator**: a function returning an plain action object
```
function showNotification(id, text) {
  return { type: 'SHOW_NOTIFICATION', id, text }
}
function hideNotification(id) {
  return { type: 'HIDE_NOTIFICATION', id }
}

// dispatch asynchorously
store.dispatch(showNotification('You just logged in.'))
setTimeout(() => {
  store.dispatch(hideNotification())
}, 5000)
```

## Dispatch an extracted async function
```
// actions.js
// ...
let nextNotificationId = 0
export function showNotificationWithTimeout(dispatch, text) {
  const id = nextNotificationId++
  dispatch(showNotification(id, text))

  setTimeout(() => {
    dispatch(hideNotification(id))
  }, 5000)
}

// component.js
showNotificationWithTimeout(this.props.dispatch, 'You just logged in.')
```
-> dispatch the function with **dispatch** as callback -> not action creator

# With Thunk
## Async Action Creator (Thunk)
A function returning a function with **dispatch** and **getState** as argument, used like normal action creators. But unlike in a regular action creator, we can exit early in a thunk, and Redux doesn’t care about its **return value** (or lack of it).  
```
export function showNotificationWithTimeout(text) {
  return (dispatch) => {
    const id = nextNotificationId++
    dispatch(showNotification(id, text))

    setTimeout(() => {
      dispatch(hideNotification(id))
    }, 5000)
  }
}

this.props.dispatch(showNotificationWithTimeout('You just logged in.'))
```

## Prevent side effects under some condition
```
export function showNotificationWithTimeout(text) {
  return (dispatch, getState) => {
    if (!getState().areNotificationsEnabled) {
      return
    }

    // API call and dispatch
  }
}
```
-> To prevent executing API calls when cached data available, not to **conditionally dispatch different actions** (do that in reducers)

## Return Promise from Thunk
The **return value** of dispatch(thunk) is the return value of the inner function of the thunk, so it's useful to return a Promise:  
`dispatch(getUser(25)).then(...)`

```
getUser = (id) => {
  return dispatch => {
    dispatch({ type: 'GET_USER_REQUEST', id })

    return fetchUserAPI().then(
      response => {
        dispatch({ type: 'GET_USER_SUCCESS', id,  response })
      },
      error => {
        dispatch({ type: 'GET_USER_FAILURE', id,  error })
        // Rethrow so returned Promise is rejected
        throw error
      }
    )
  }
}
```

Return a Promise from a **sync** action: `return async (dispatch) => ...`

## Chain Async Action Creators
getPost() is another thunk returning promise  
getUserAndTheirFirstPost() is a thunk combining the two thunks, also returns a promise (the promise returned from getPost), so thenable
```
getUserAndTheirFirstPost = (userId) => {
  return (dispatch, getState) => {
    return dispatch(getUser(userId)).then(() => {
      const fetchedUser = getState().usersById[userId]
      const firstPostID = fetchedUser.postIDs[0]
      return dispatch(getPost(firstPostID))
    })
  }
}

store.dispatch(getUserAndTheirFirstPost(43)).then(() => {
  console.log('fetched a user and their first post')
})
```
