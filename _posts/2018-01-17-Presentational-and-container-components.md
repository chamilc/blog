---
title: Presentational and container components
author: Anuradha Malalasena
linkedin: https://lk.linkedin.com/in/bamalalasena
description : Presentational, container, components, React
---

[Presentational and container
components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)
is a pattern introduced by Dan Abramov, React core team member, and
co-author of both Redux and React-Redux libraries. This pattern is very
useful when developing React applications. Components are much easier to
reuse and reason if you divide them into two parts.

**Presentational Component** 

Presentational Component is a component that just renders HTML. The
component's only function is presentational markup. In a Redux powered
app, a presentational component does not interact with the Redux store.

**Presentational Component Characteristics**

1.  Describe how things look

2.  Have no dependencies on the rest of the app. (with Flux actions
    or stores.)

3.  Receive only props (providing data and callbacks)

4.  Rarely have own state (when they do, it’s just UI state)

**Container component**

The container component specifies the data a presentational component
should render. The container component also specifies behavior. If the
presentational component has any actions, like a button click, it calls
a prop-function given to it by the container component. The container
component is the one to dispatch an action to the Redux store.

**Container Component Characteristics**

1.  Describe how things work

2.  Provide no DOM markup or styles

3.  Are often stateful, as they tend to serve as data sources.

4.  Call Flux actions

**Benefits of This Approach**

1.  Better separation of concerns. You understand your app and your UI
    better by writing components this way.

2.  Better reusability. You can use the same presentational component
    with completely different state sources.

3.  Presentational components are essentially your app’s “palette”. You
    can let the designer tweak all their variations without touching the
    app’s logic

**Sample Codes**

Presentational Component

```javascript
import React, { Component } from 'react'
import NewsCardView from './NewsCardView'

class NewsGridView extends Component {

    componentWillMount() {
        this.props.getNews(this.props.sources, this.props.category);
    }

    render() {
        return (            
            <div className="row">
                <div className="col s10">
                    {this.props.articles.current.map((article, index) => (
                        <div key={index}>
                            <NewsCardView article={article} />
                        </div>
                    ))}

                </div>
            </div>
        )
    }
}

export default NewsGridView
```

Container component

```javascript
import React from 'react'
import { connect } from 'react-redux'
import NewsGridView from '../components/NewsGridView'

const mapStateToProps = (state, props) => {
  return {
    articles: state.articles
  }
}

const mapDispatchToProps = dispatch => {
  return {
    getNews: (sources, category) => {
      dispatch(apiActions.getNews(sources, category))
    }
  }
}

const NewsGrid = connect(
  mapStateToProps,
  mapDispatchToProps
)(NewsGridView)

export default NewsGrid
```
