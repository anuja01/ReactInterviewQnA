### 1. How to store data locally within react native app.  
`async-storage` stores unencrypted, key-value data. Do not use Async Storage for storing Token, Secrets and other confidential data.   
Do use Async Storage for persisting Redux state, GraphQL state and storing global app-wide variables.   
https://github.com/react-native-community/async-storage.  

### 2. How to store sensitive data like auth token within react native app.  
`react-native-keychain` stores username/password combination in the secure storage in string format. Use JSON.stringify/JSON.parse when you store/access it.   
https://github.com/oblador/react-native-keychain.     

### 3. How to check for an Internet Connection in a React Native App.  
`@react-native-community/netinfo` - React Native Network Info API for Android, iOS, macOS & Windows. It allows you to get information on:   
* Connection type
* Connection quality
