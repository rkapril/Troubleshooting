# Troubleshooting

## 1. Overrun of a stack based buffer

### Windows

![system-detected-an-overrun-of-a-stack-based-buffer](/overrun-of-a-stack-based-buffer/windows/system-detected-an-overrun-of-a-stack-based-buffer.jpg "system-detected-an-overrun-of-a-stack-based-buffer")

- Run SFC/DISM

  - Type cmd, and select Run as administrator under Command Prompt.

    ```
    DISM /online /cleanup-image /scanhealth
    ```

    ```
    DISM /online /cleanup-image /restorehealth
    ```

    ```
    sfc /scannow
    ```

- Microsoft Safety Scanner

## 2. Blocked by CORS policy

![blocked-by-CORS-policy](/blocked-by-CORS-policy/blocked-by-CORS-policy.PNG "blocked-by-CORS-policy")

### Solution:

Create a Proxy server in React App

1. Create a `server` folder in the root folder

2. Create `proxy-server.mjs` in the `server` folder

proxy-server.mjs

```
import express from "express";
import cors from "cors";
import fetch from "node-fetch";

const app = express();
app.use(cors());

app.get("/api", async (req, res) => {
  try {
    const response = await fetch("http://randomuser.me/api/?results=1");
    const data = await response.json();
    res.json(data);
  } catch (error) {
    res.status(500).json({ error: "Something went wrong" });
  }
});

const PORT = 3001; // Choose a port number that is not being used
app.listen(PORT, () => {
  console.log(`Proxy server listening on port ${PORT}`);
});
```

App.js

```
import React from "react";

function App() {
  const [user, setUser] = React.useState([]);

  const fetchData = () => {
    fetch("http://localhost:3001/api")
      .then((response) => response.json())
      .then((data) => {
        setUser(data);
        console.log(data);
      });
  };

  React.useEffect(() => {
    fetchData();
  }, []);

  return Object.keys(user).length > 0 ? (
    <div>
      <h1>Data returned</h1>
      <h2>First Name: {user.results[0].name.first}</h2>
      <h2>Last Name: {user.results[0].name.last}</h2>
    </div>
  ) : (
    <h1>Data pending...</h1>
  );
}

export default App;
```

3. `npm install express node-fetch cors`

4. `node proxy-server.mjs` from the server folder

5. `npm start` from the root folder to start React App
