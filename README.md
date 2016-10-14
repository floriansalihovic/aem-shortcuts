# AEM Shortcuts
Some things are just too trouble some to remember and Adobe's site is too slow ... so, this is more or less a structured, documented big GIST ...

## Client libraries

Client libaries are defined - you already guessed it - in a `.content.xml` as follows:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:ClientLibraryFolder"
          categories="[]"
          dependencies="[]"
          embed="[]"/>
```

The `categories` provided a way of associating different client libs with each other. `dependencies` are required but loaded seperately while embedded libs become part of the big one.

### Widgets

Custom widgets are usually the last choice of task to be dealt with, because `Ext` is a beast of its own. Here is a scaffold for a custom `xtype`:

```javascript
(function (CQ) {

  /**
   * Extending the CQ.form.Selection component for whatever reason.
   */
  var UberSelection = CQ.Ext.extend(CQ.form.Selection, {
    constructor: function (config) {
      UberSelection.superclass.constructor.call(this, CQ.Util.applyDefaults(config || {}, defaults));
    }
  });

  CQ.Ext.reg('uberselection', UberSelection);
}(CQ));
```

### Loading predefined nodes in JavaScript for Dialogs

Usually CQ includes are used to stay `DRY` - but sometime, in rare occasions, you need those includes multiple times in you dialog. That's when you need to break the limitations of AEM.

```javascript
/**
 * http://stackoverflow.com/questions/32472702/how-to-lazy-load-tags-in-cq-dialog
 *
 * Refer to the stackoverflow post for layout issues.
 */ 
(function(CQ) {
  
  /**
   * Loads nodes from the repository and translates them into meaningful
   * data structures.
   */
  function include(path) {
    // local variables for debugging purposes
    var data = CQ.shared.HTTP.eval(path + '.infinity.json');
    var defaults = CQ.Util.formatData(data);
    return defaults;
  }

  return {
    include: include
  }
}(CQ));
```

