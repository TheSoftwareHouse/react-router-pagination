# Status

[![Build Status](https://travis-ci.org/TheSoftwareHouse/react-router-pagination.svg?branch=master)](https://travis-ci.org/TheSoftwareHouse/react-router-pagination) [![Code Coverage](https://codecov.io/gh/TheSoftwareHouse/react-router-pagination/branch/master/graph/badge.svg)](https://codecov.io/gh/TheSoftwareHouse/react-router-pagination) [![License](https://img.shields.io/npm/l/@tshio/react-router-pagination.svg)](https://github.com/TheSoftwareHouse/react-router-pagination/blob/master/LICENSE.md) [![Version](https://img.shields.io/npm/v/@tshio/react-router-pagination.svg)](https://www.npmjs.com/package/@tshio/react-router-pagination)

# react-router-pagination

## Installation

Using [yarn](https://yarnpkg.com/lang/en/):

    $ yarn add @tshio/react-router-pagination

Using [npm](https://www.npmjs.com/):

    $ npm install --save @tshio/react-router-pagination

## Usage

In order to connect custom component with pagination, it is required to import [`withRouter`](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/withRouter.md) function from [`react-router-dom`](https://github.com/ReactTraining/react-router/tree/master/packages/react-router-dom) and use it to wrap `connectRouterWithPagination` function.

At the beginning, wrap your list component using `connectRouterWithPagination` function in your container:

```js
import { withRouter } from 'react-router-dom';
import { connect } from 'react-redux';
import { compose } from 'redux';

import { connectRouterWithPagination } from '@tshio/react-router-pagination';

import { fetchUsers } from '../redux/users/users.actions';
import { ListComponent } from '../list.component';
import { PaginationComponent } from '../pagination.component';

const mapStateToProps = () => ({ ... })

const mapDispatchToProps = dispatch => ({
    onPageChange: (params) => dispatch(fetchUsers(params)) // `onPageChange` method will dispatch your redux action when page changes
});

//And now, connect your store with list component

export const ListContainer = compose(
  withRouter,
  connect(mapStateToProps, mapDispatchToProps),
  connectRouterWithPagination(PaginationComponent,
    // In first parameter pass your custom pagination component
    {
        // Second parameter (optional) is an object and allow you to pass config options
        pageChangeCallbackKey: "pageChangeCallback",
        currentPageKey: "page",
        pageParamName: "usersListPage",
        itemsPerPageParamName: "itemsNumber",
        defaultItemsPerPage: 8
        // See list of available options below..
    }
  )
)(ListComponent);
```

**Keep in mind, that `onPageChange` function is required and always has to be implemented!**

And then, you can render pagination component in list component:

```js
import React, { Component } from 'react';

// ...

export class ListComponent extends Component {
  render() {
    return (
      <div>
        // ...
        {this.props.pagination({
          // here, you can define custom properties for pagination component
        })}
      </div>
    );
  }
}
```

This package is working with `page` and `itemsPerPage` parameters which are included in URL. If `page` param is missing, it will be set as default with value `1`. If `itemsPerPage` param is missing, it will be initialized with value of `defaultItemsPerPage` from config options or with value of `10`.

## Config options

| Option name           | Default value  | Type     | Role                                                                                                   |
| --------------------- | -------------- | -------- | ------------------------------------------------------------------------------------------------------ |
| currentPageKey        | `currentPage`  | `string` | Name of prop which is passed to pagination component to point on current page.                         |
| pageChangeCallbackKey | `onChange`     | `string` | Name of callback function which is called when you click on navigation button in pagination component. |
| pageParamName         | `page`         | `string` | Determines what is the name of page param which is displayed in URL; e.g. `/users?page=1`.             |
| itemsPerPageParamName | `itemsPerPage` | `string` | Determines what is the name of items per page param displayed in URL; e.g. `/users?itemsPerPage=10`    |
| defaultItemsPerPage   | `10`           | `number` | Determines initial and default value of number of items per page.                                      |

## Development

We welcome all contributions. Please read our [CONTRIBUTING.md](https://github.com/TheSoftwareHouse/react-router-pagination/blob/master/CONTRIBUTING.md) first.
You can submit any ideas as [GitHub issues](https://github.com/TheSoftwareHouse/react-router-pagination/issues).
