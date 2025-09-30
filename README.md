# <img src="./docs/img/indom.svg" alt="InDom" width="147" height="57"> - 3.7KB modern JavaScript DOM library - powerful, easy and automates cleanup

![quick taste example 1](./docs/img/readme-quick-taste-1b.jpg)![quick taste example 2](./docs/img/readme-quick-taste-2b.jpg)

- **Lightweight:** Only **3.7KB gzipped** – adds minimal overhead.

- **Complete Innovative DOM Solution:** Comprehensive API with single-instance objects per element – eliminating duplication and boosting performance for element selection, manipulation, traversal, event handling and more. 

- **Modern JavaScript:** Built with ES2022, empowers clean and maintainable code.

- **Powerful Cleanup:** State (event listeners, data, etc.) **automatically** removed when elements are destroyed 

- **Stack Agnostic:** Leak-proof by design, no need for manual cleanup to avoid memory leaks. Set events with InDom, remove elements with any library (an older JS DOM lib, a modern large JS framework, etc.) – cleanup still happens automatically

- **Fast & Dependency-Free:** Optimized for performance with zero external dependencies.

- **Modern Browser Support:** Compatible with all modern browsers (see browser support for more).

- **Powerful Fields Handling:** Advanced form field handling with automatic type normalization and validation.

- **Multiple Formats:** Distributed in Plain JavaScript, ESM modules, and TypeScript formats.

- **TypeScript Ready:** Includes built-in types (ES2022 compatible) available in `/src`.



## Table of Contents
- **API**
  - [Shortcuts for convenience](#shortcuts-for-convenience)
  - [InDom.getOne](#indom-getone-selector-container) - **$1**
  - [InDom.get](#indom-get-selector-container) - **$a**

	- [InDom.getById](#indom-getbyid-id) - **$id**

	- [new InDom](#new-indom-source) - **$n**

⚬ [InDom.onReady](#indom-onready-fn)

⚬ [.getValue](#getvalue-container)

⚬ [InDom.getValues](#indom-getvalues-args) - <span style="color:#04281c;text-shadow:0px 0px 1px;">$v</span>

⚬ [.setValue](#setvalue-value-container)

⚬ [.on + .onClick , onEnter etc.](#on-type-fn-opts)

⚬ [.onRemove](#onremove-fn)

⚬ [.off](#off-type-fn)

⚬ [.setData](#setdata-key-value)

⚬ [.getData](#getdata-key)

⚬ [.hasData](#hasdata-key)

⚬ [.removeData](#removedata-key)

⚬ [.getElement / .el](#getelement-el)

⚬ [.remove](#remove)

⚬ [.is](#is-selector)

⚬ [.getParent](#getparent-selector)

⚬ [.getSelfOrParent](#getselforparent-selector)

⚬ [.getNext](#getnext-selector)

⚬ [.getPrev](#getprev-selector)

⚬ [.setHtml](#sethtml-content)

⚬ [.getHtml](#gethtml)

⚬ [.append](#append-children)

⚬ [.prepend](#prepend-children)

⚬ [.after](#after-siblings)

⚬ [.before](#before-siblings)

⚬ [.setAttr](#setattr-key-value)

⚬ [.getAttr](#getattr-key)

⚬ [.hasAttr](#hasattr-key)

⚬ [.removeAttr](#removeattr-key)

⚬ [.getBox](#getbox)

⚬ [.getOuterBox](#getouterbox)

⚬ [.getRelativeBox](#getrelativebox)

⚬ [.addClass](#addclass-names)

⚬ [.hasClass](#hasclass-name)

⚬ [.removeClass](#removeclass-names)

⚬ [.setStyle](#setstyle-property-map-value)

⚬ [.getStyle(...properties?)](#getstyle-properties)