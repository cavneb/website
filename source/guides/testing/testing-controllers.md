By the end of this document, the reader should understand:

* when and when not to test controllers (unit vs integration)
* how to test controllers (integration)
* how to test controllers (unit)

Unit testing controllers is very simple using the unit test helper `moduleFor`.

***It can become very easy to let your controllers perform most of the business logic in an Ember application. It is considered best practice to extract out any logic that does not directly impact the template into it's own library.***



<a class="jsbin-embed" href="http://jsbin.com/gahoy/4/embed?output">Unit Testing Controllers</a>
<script src="http://static.jsbin.com/js/embed.js"></script>