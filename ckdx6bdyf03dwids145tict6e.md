## TIL: Abort Controller

Usually Async requests are not cancellable and you just have to abandon those requests.

Today I learned about the Abort Controller and it is very useful. 

It has a really good support in general and if you are using Babel with CoreJS, I thing it will be transpiled anyway.

Let's take a look at an example. Let's create a simple Image Uploader in plain HTML, CSS and JS.

HTML:
```html
  <form id="imageUploadForm">
    <div>
      <label for="file">Image</label>
      <div>
        <input type="file" name="file" id="file" />
      </div>
    </div>
    <div>
      <button type="submit">Start Upload</button>
      <button type="reset">Reset</button>
      <button type="button" id="stop">Stop Upload</button>
    </div>
  </form>
```

And JavaScript:
```js
document.addEventListener('DOMContentLoaded', () => {
  const formEl = document.querySelector('#imageUploadForm')
  formEl && attachFormEvents(formEl)
})

const attachFormEvents = (formEl) => {
  if (!formEl) {
    return
  }
  const stop = formEl.querySelector('#stop')
  const controller = new AbortController()
  const signal = controller.signal

  formEl.addEventListener('submit', (e) => {
    e.preventDefault()
    e.stopPropagation()
    const formData = new FormData(formEl)
    const url = 'https://1u3o3.sse.codesandbox.io/upload'
    return fetch(url, {signal, method: 'POST', body: formData})
      .then(r => r.json())
      .then(console.log)
      .catch(console.error)
  })

  stop && stop.addEventListener('click', (e) => {
    controller.abort()
  })
}
```

How it works? Lets take a look at it step by step:
1. Create an Abort Controller with `new AbortController()`
2. Get the signal from the Controller with `controller.signal`
3. When creating `fetch`, provide abort signal in the Request Configuration with key `signal`
4. Call `controller.abort()` whenever you wanna abort the request

----

Let me know if you have tried the Abort Controller already.

