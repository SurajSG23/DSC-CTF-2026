## Endgame Protocol 🫰⚖️

### Challenge Description

The *Endgame Protocol* challenge presents a mysterious **Snap** button inspired by Thanos’ famous quote:
**“Perfectly balanced. As all things should be.”**
Clicking the button outputs a strange string of unreadable characters. The goal is to **reverse the snap** and recover the original flag 🚩.

---

### Hint 💡

The challenge title and description strongly hint at **reversing a transformation** rather than brute-forcing. Since this is a web-based challenge, the logic is likely implemented in **client-side JavaScript** 🧠💻.

---

### Challenge Link 🌐

`https://endgame.challenges1.ctf.dscjssstuniv.in/`

---

### Initial Observation 👀

On opening the website, a **Snap** button is displayed.
When clicked, it sends a request to the `/snap` endpoint and prints an encoded output:

```
ESBAQD~ta;Vz@:cQ$wu!y&Ip.0Eiv.AQTXGLx145
```

This output does not resemble a flag and clearly appears to be **encoded or obfuscated**.

---

### Approach and Analysis 🔍

I inspected the page source and noticed a JavaScript file named:

```
/static/protocol.min.js
```

Inside this file, I found the following function:

```js
function p(r) {
  let o = "";
  for (let i = 0; i < r.length; i++) {
    o += String.fromCharCode((r.charCodeAt(i) ^ (i % 42)) + 1);
  }
  return o;
}
```

From this, I understood:

* Each character is XORed with `(i % 42)`
* Then incremented by `+1`
* The output shown on the page is the **encoded version** of the flag

To retrieve the original flag, these operations must be **reversed**.

---

### Exploitation Steps ⚙️

To reverse the encoding:

1. Subtract `1` from each character’s ASCII value
2. XOR it again with `(i % 42)`

I wrote the following decoding script in the browser console:

```js
let enc = "ESBAQD~ta;Vz@:cQ$wu!y&Ip.0Eiv.AQT\u0013X\u0015G\u0017Lx\u001c145\u007f";

let flag = "";
for (let i = 0; i < enc.length; i++) {
  flag += String.fromCharCode((enc.charCodeAt(i) - 1) ^ (i % 42));
}

console.log(flag);
```

---

### Result 🎉

Running the script successfully decoded the original flag and printed it in readable form.

---

### Flag 🚩

```
DSCCTF{th3_r34l_3ndg4m3_w45_th3_pr0t0c0l_2026}
```
