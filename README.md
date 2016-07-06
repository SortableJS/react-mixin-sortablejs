# THIS PROJECT NEEDS A MAINTAINER.

---

react-mixin-sortablejs
-------------------
React mixin for [SortableJS](https://github.com/RubaXa/Sortable/).

Demo: http://rubaxa.github.io/Sortable/


<a name="react"></a>
### Support React
Include [react-sortable-mixin.js](react-sortable-mixin.js).
See [more options](react-sortable-mixin.js#L26).

```jsx
var SortableList = React.createClass({
	mixins: [SortableMixin],

	getInitialState: function() {
		return {
			items: ['Mixin', 'Sortable']
		};
	},

	handleSort: function (/** Event */evt) { /*..*/ },

	render: function() {
		return <ul>{
			this.state.items.map(function (text, i) {
				return <li ref={i}>{text}</li>
			})
		}</ul>
	}
});

ReactDOM.render(<SortableList />, document.body);


//
// Groups
//
var AllUsers = React.createClass({
	mixins: [SortableMixin],

	sortableOptions: {
		ref: "user",
		group: "shared",
		model: "users"
	},

	getInitialState: function() {
		return { users: ['Abbi', 'Adela', 'Bud', 'Cate', 'Davis', 'Eric'] };
	},

	render: function() {
		return (<div>
				<h1>Users</h1>
				<ul ref="user">{
					this.state.users.map(function (text, i) {
						return <li ref={i}>{text}</li>
					})
				}</ul>
			</div>
		);
	}
});

var ApprovedUsers = React.createClass({
	mixins: [SortableMixin],
	sortableOptions: { group: "shared" },

	getInitialState: function() {
		return { items: ['Hal', 'Judy'] };
	},

	render: function() {
		return <ul>{
			this.state.items.map(function (text, i) {
				return <li ref={i}>{text}</li>
			})
		}</ul>
	}
});

ReactDOM.render(<div>
	<AllUsers/>
	<hr/>
	<ApprovedUsers/>
</div>, document.body);
```

### Support React ES2015 / TypeScript syntax
As mixins are not supported in ES2015 / TypeScript syntax here is example of ES2015 ref based implementation.
Using refs is the preferred (by facebook) "escape hatch" to underlaying DOM nodes: [React: The ref Callback Attribute](https://facebook.github.io/react/docs/more-about-refs.html#the-ref-callback-attribute)

```js
import * as React from "react";
import Sortable from 'sortablejs';

export class SortableExampleEsnext extends React.Component {

  sortableContainersDecorator = (componentBackingInstance) => {
    // check if backing instance not null
    if (componentBackingInstance) {
      let options = {
        handle: ".group-title" // Restricts sort start click/touch to the specified element
      };
      Sortable.create(componentBackingInstance, options);
    }
  };

  sortableGroupDecorator = (componentBackingInstance) => {
    // check if backing instance not null
    if (componentBackingInstance) {
      let options = {
        draggable: "div", // Specifies which items inside the element should be sortable
        group: "shared"
      };
      Sortable.create(componentBackingInstance, options);
    }
  };

  render() {
    return (
      <div className="container" ref={this.sortableContainersDecorator}>
        <div className="group">
          <h2 className="group-title">Group 1</h2>
          <div className="group-list" ref={this.sortableGroupDecorator}>
            <div>Swap them around</div>
            <div>Swap us around</div>
            <div>Swap things around</div>
            <div>Swap everything around</div>
          </div>
        </div>
        <div className="group">
          <h2 className="group-title">Group 2</h2>
          <div className="group-list" ref={this.sortableGroupDecorator}>
            <div>Swap them around</div>
            <div>Swap us around</div>
            <div>Swap things around</div>
            <div>Swap everything around</div>
          </div>
        </div>
      </div>
    );
  }
}
```
