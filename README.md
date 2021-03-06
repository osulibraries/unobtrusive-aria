unobtrusive-aria
================

Keep WAI-ARIA events/markup separate from HTML using jQuery


# Example

## Before:

```html
<ul role="menubar">
	<li role="presentation">
		<a role="menuitem" tabindex="0" href="/home">Home</a>
	</li>
	<li role="presentation">
		<a aria-haspopup="true" role="menuitem" href="/about">About</a>
		<ul>
			<li role="presentation">
				<a href="/about/departments" role="menuitem" tabindex="-1">Departments</a> 
			</li>
		</ul>
	</li>
	<li role="presentation">
		<a aria-haspopup="true" role="menuitem" href="/contact">Contact</a>
		<ul>
			<li role="presentation">
				<a href="/contact/email-us" role="menuitem" tabindex="-1">Email Us</a> 
			</li>
		</ul>
	</li>
</ul>
```

## After:
```html
<ul id="navigation">
  <li><a href="/home">Home</a></li>
  <li><a href="/about">About</a>
    <ul class="sub">
      <li><a href="/about/departments">Departments</a></li>
    </ul>
  </li>
  <li><a href="/contact">Contact</a>
    <ul class="sub">
      <li><a href="/contact/email-us">Email Us</a></li>
    </ul>
  </li>
</ul>
```
```javascript
$(document).ready(function(){
	$(document).unobtrusiveAria({
		'#navigation > ul':{
        attributes:{
            role:'menubar'
        }
    },
    '#navigation > ul > li':{
        attributes:{
            role:'presentation'
        },
        events:{
            keydown:function (event) {
                switch (event.keyCode) {
                    case $.ui.keyCode.LEFT:
                    $(this).prev('li').find('>a').focus();
                    event.preventDefault();
                    break;
                    case $.ui.keyCode.RIGHT:
                    $(this).next('li').find('>a').focus();
                    event.preventDefault();
                    break;
                    case $.ui.keyCode.DOWN:
                    $(this).find('ul > li:first > a').focus();
                    $(this).find('ul').attr('aria-expanded',true);
                    event.preventDefault();
                    break;
                }
                ;
            }
        }
    },
    '#navigation > ul > li > a': {
        attributes: {
            'aria-haspopup': true
        }
    },
    '#navigation ul.sub > li > a':{
        attributes:{
            role:'menuitem',
            tabindex: -1
        }
    },
    '#navigation > ul > li:first > a':{
        attributes:{
            role:'menuitem',
            tabindex: 0
        }
    },
    '#navigation ul.sub > li': {
        attributes: {
            role:'presentation'

        },
        events:{
            keydown:function (event) {
                switch (event.keyCode) {
                    case $.ui.keyCode.LEFT:
                    $(this).prev('li').find('>a').focus();
                    event.preventDefault();
                    event.stopPropagation();
                    break;
                    case $.ui.keyCode.RIGHT:
                    $(this).next('li').find('>a').focus();
                    event.preventDefault();
                    event.stopPropagation();
                    break;
                    case $.ui.keyCode.DOWN:
                    $(this).next('li').find('>a').focus();
                    event.preventDefault();
                    event.stopPropagation();
                    break;
                    case $.ui.keyCode.UP:
                    if($(this).prev('li').length < 1){
                        $(this).parent('ul.sub').attr('aria-expanded',false);
                        $(this).parents('li').find('>a').focus();
                    }
                    else{
                        $(this).prev('li').find('>a').focus();
                    }
                    event.preventDefault();
                    event.stopPropagation();
                    break;
                }
                ;
            }
        }
    }

	});
});
```