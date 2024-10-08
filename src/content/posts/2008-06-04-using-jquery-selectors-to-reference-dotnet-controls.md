---
title: Using jQuery selectors to reference .Net controls
date: 2008-06-04
published: true
tags: ["DotNet", "JavaScript"]
canonical_url: false
description: ""
---

You place a control onto your page:

```html
<asp:Content ID="Content1" ContentPlaceHolderID="Content" Runat="Server">
  <asp:TextBox Id="Firstname" Runat="Server" />
</asp:Content>
```

The output is rendered as:

```html
<input
  name="ctl00$Content$FirstName"
  type="text"
  id="ctl00_Content_FirstName"
/>
```

In the example above, the disparity in the rendered id attribute occurs because the TextBox control has been placed within a Content control, as the Page object inherits a MasterPage.

So how can you robustly reference the control using client-side JavaScript, regardless where the control resides?Â Using jQuery it's fairly trivial:

```javascript
var $firstname = $("[id$=FirstName]");
```

This little snippet makes use of the jQuery attributeEndsWith selector, as documented [here](https://jquery.com/#attributevalue).Â Bare in mind that an array is returned, as any control with an id that ends in FirstName will be selected.

## One thought on “Using jQuery selectors to reference .Net controls”

### Adam

_January 3, 2011 at 1:06 pm_

Use could use inline tags to place the ClientID.

```javascript
var $firstname = $["#<%= Firstname.ClientID %>"]);
```

This would insert the ClientID into your javascript code block and help find the control on the page.

---
