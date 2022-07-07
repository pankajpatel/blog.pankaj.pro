## 🚦Submitting a Form to different Actions in HTML5

For so long, I had believed that one Form can only be submitted to one Action URL without any JS.

> I couldn’t be more wrong.

Though, lets take a look at the way of the Past i.e. with JavaScript and AJAX/Fetch

---

## Problem

Consider following HTML Form where we want to add or update addresses to the User Profiles.

```html
<form id='addressForm'>
  ...
  <!-- Form Fields -->
  ...
  <button id='addAddress' class="btn btn-primary">Add Address</button>
  <button id='updateAddress' class="btn btn-success">Update Address</button>
</form>
```

In the above form, there are submit buttons to `Add Address` or `Update Address` Though they will only send `POST` request to `/addAddress` .

Which means the Update Address will still lead to adding a new address rather than updating the current one.

To make this work, we will need to use JavaScript.

Now with JavaScript, there are multiple ways to do so. One simple way is to submit form via AJAX request

Let’s see how to submit the above Form to different Action endpoints with AJAX:

```js
document.addEventListener('DOMContentLoaded', () => {
  const form = document.querySelector('#addressForm')
  const addButton = document.querySelector('#addAddress')
  const updateButton = document.querySelector('#updateAddress')

  addButton && addButton.addEventListener('click', (e) => {
    e.preventDefault()

    if (!form.checkValidity()) {
      return
    }

    fetch('/addAddress', { method: 'POST', body: new FormData(form) })
      .then((r => r.json()))
      .then(console.log)
  })
  
  updateButton && updateButton.addEventListener('click', (e) => {
    e.preventDefault()

    if (!form.checkValidity()) {
      return
    }

    fetch('/updateAddress', { method: 'POST', body: new FormData(form) })
      .then((r => r.json()))
      .then(console.log)
  })
})
``` 

Now as we can see that with JavaScript the HTML and deform submitting is very tightly coupled.

This old way of sending Form to multiple endpoints can be checked on https://multi-action-forms.netlify.app/with-ajax.html

---

> Now what is the solution?

## Solution

I came across these attributes for the submit buttons, where you can specifically make the form to be submitted on different URL with different HTTP method and encoding type.

Those attributes are fromaction, formmethod and formenctype.

With these attributes, we can go back to adding a normal submit button and default attributes to the form.

Along with that, we will add another submit button with attributes mentioned in the previous line.

Here’s how new HTML Form will look like:
```html
<form action="/addAddress" method="POST">
  ...
  <!-- Form Fields -->
  ...
  <button type="submit" class="btn btn-primary">Add</button>
  <button type="submit" class="btn btn-success" formaction="/updateAddress" formmethod="POST">Update</button>
</form>
```

Not just on Buttons, but on input of type button also accepts these attributes to allow sending Form Submissions to different endpoints.

---

## Benefits

* No need to attach JS for basic functionalities
* HTML5 Validation API can be leveraged with its normal flow
* Form can work in Pages with JS disabled (with exceptions)

---

## Concerns
* Browser Support, the old browsers might not work with this and hence you might need Polyfills/Shims
* Modern Front End apps like Single Page Applications, MicroFrontends etc might need some extra tuning with work with this

---

[Github Repo](https://github.com/time2hack/multi-action-forms-example) ・ [Demo](https://multi-action-forms.netlify.app/)

---

## Conclusion

As we saw how we can utilize the Browser’s inbuilt APIs to get more things done with less code.

> Would you use in you application? Why or Why not?

Let me know through comments 💬 or on Twitter at  [@patel_pankaj_](https://twitter.com/intent/follow?screen_name=patel_pankaj_)  and/or  [@time2hack](https://twitter.com/intent/follow?screen_name=time2hack) 

If you find this article helpful, please share it with others 🗣

Subscribe to the blog to receive new posts right to your inbox.

---

#### Credits

* Icon from [Iconfinder](https://www.iconfinder.com/icons/6860341/expand_form_open_widget_icon)
* Photo by  [Mitchell Luo](https://unsplash.com/@mitchel3uo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  on  [Unsplash](https://unsplash.com/s/photos/developer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 

---

Originally published at [https://time2hack.com](https://time2hack.com/single-form-with-multiple-actions/) on Jan 13, 2021.

