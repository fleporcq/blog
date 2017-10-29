+++
# Hero widget.
widget = "hero"
active = true
date = 2017-10-15

title = ""

# Order that this section will appear in.
weight = 1

# Overlay a color or image (optional).
#   Deactivate an option by commenting out the line, prefixing it with `#`.
[header]
  overlay_color = "#666"  # An HTML color value.
  overlay_img = "headers/bubbles-wide.jpg"  # Image path relative to your `static/img/` folder.
  overlay_filter = 0.5  # Darken the image. Value in range 0-1.
+++

<span id="quote"></span>&nbsp;<small id="author"></small>

<script type="text/javascript">
  (function defer() {
    if (window.jQuery) {
      jQuery(document).ready(function(){
        displayRandomQuote();
      });
    } else {
      setTimeout(function() { defer() }, 50);
    }
  })();  
  function displayRandomQuote() {
    $.getJSON('/resources/quotes.json').done(function (quotes) {
      var quote = quotes[Math.floor(Math.random() * quotes.length)];
      $('#quote').text(quote.quote);
      if(quote.author != ""){
        $('#author').text("- " + quote.author);
      }
    });    
}  
</script>