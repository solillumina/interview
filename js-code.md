# JS: Fetch file as FormData

```js
const formData = new FormData(); // Require header Content-Type: multipart/form-data (fetch will auto set the header)
formData.append("file", e.target.files[0]);
// formData.append("file", e.target.files[1]); // Can set multiple file like this
fetch("https://example.com/api/upload", {
  method: "POST",
  body: formData,
});
```

# JS: Read file

```js
const text = await e.target.files[0].text();
// await e.target.files[0].text()
// await e.target.files[0].byte() // Uint8Array (1 byte per index)
// await e.target.files[0].arrayBuffer() // Fix length binary

// Old way
const reader = new FileReader();
let text2 = "";
reader.onload = () => {
  text2 = reader.result;
};
reader.readAsText(e.target.files[0]);
// reader.readAsArrayBuffer(e.target.files[0]); // Array buffer
// reader.readAsDataURL(e.target.files[0]); // Base64
```

# NodeJS: Handle Post

```js
const http = require("http");
const node = http.createServer((req, res) => {
  if (req.url === "/path" && req.method.toLowerCase() === "post") {
    let body = "";

    req.on("data", (chunk) => {
      body += chunk.toString();
    });

    req.on("end", () => {
      try {
        const data = JSON.parse(body); // Assuming the client sent JSON

        res.writeHead(200, { "Content-Type": "application/json" });
        res.end(JSON.stringify({ status: "success", received: data }));
      } catch (err) {
        res.writeHead(400);
        res.end("Invalid JSON");
      }
    });
  }
});

server.listen(3000, () => console.log("Server running on port 3000"));
```
