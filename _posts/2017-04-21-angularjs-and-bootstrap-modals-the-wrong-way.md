---
layout: post
title: AngularJS and Bootstrap Modals the Wrong Way
---

Bootstrap and AngularJS are great frameworks, and it makes sense that people would want to use them together. But there are a number of ways to go astray when using them together for the first time. One example is when using Bootstrap's modal. 

One traditional way to use Bootstrap's modal is by using `data-toggle` and `data-target` combinations. You'd write the modal HTML, give it an `id`, and pass the `id` to the `data-target`. On the [Bootstrap 3 page](http://getbootstrap.com/javascript/#modals), an example modal is given something like this

```html
<div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel">
  <div class="modal-dialog" role="document">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-label="Close"><span aria-hidden="true">&times;</span></button>
        <h4 class="modal-title" id="exampleModalLabel">My Modal</h4>
      </div>
      <div class="modal-body">
        <!-- Modal content -->
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Send message</button>
      </div>
    </div>
  </div>
</div>

<!-- This button opens the modal -->
<button class="btn btn-default" data-toggle="modal" data-target="#exampleModal">Modal Me</button>
```

But when using AngularJS, it's generally a bad idea to call JavaScript that modifies the DOM behind Angular's back,  and that's exactly what `data-toggle` and the like do. In addition, using `data-toggle` doesn't feel like a very "Angular" way of doing things. For example, you can't open and close the modal based on your Angular code, which doesn't seem like a good thing. One example of when this can come back to bite you is if you, for example, had a link in your modal body. Say the link takes you to the home page of your AngularJS application. I've created this [demo plunk](http://embed.plnkr.co/quskvcvXwlAc6mSWfPc8/) to show what will happen.

When you click the link inside the modal, the route changes to the home page. Bootstrap's JavaScript doesn't get a chance to handle closing and cleaning up the modal classes applies, including `.modal-backdrop`. However, the modal content's HTML is ripped out of the DOM, which results in nothing but the dark backdrop being shown. 

Rather than use vanilla Bootstrap and AngularJS together, it's a better idea to use [AngularUI Bootstrap](https://angular-ui.github.io/bootstrap/) instead. 



