# Why use NgRx Store for State Management?

NgRx Store provides state management for creating maintainable, explicit applications through the use of single state and actions in order to express state changes. In cases where you don't need a global, application-wide solution to manage state, consider using [NgRx ComponentStore](guide/component-store) which provides a solution for local state management.

## When Should I Use NgRx Store for State Management?

In particular, you might use NgRx when you build an application with a lot of user interactions and multiple data sources, or when managing state in services are no longer sufficient.

A good guideline that might help answer the question, "Do I need NgRx Store?" is the
<a href="https://youtu.be/omnwu_etHTY" target="_blank">**SHARI**</a> principle:

- **S**hared: state that is accessed by many components and services.

- **H**ydrated: state that is persisted and rehydrated from external storage.

- **A**vailable: state that needs to be available when re-entering routes.

- **R**etrieved: state that must be retrieved with a side-effect.

- **I**mpacted: state that is impacted by actions from other sources.

However, realizing that using NgRx Store comes with some tradeoffs is also crucial. It is not meant to be the shortest or quickest way to write code. It also encourages the usage of many files.

It's also important to consider the patterns implemented with NgRx Store. A solid understanding of [`RxJS`](https://rxjs.dev) and [`Redux`](https://redux.js.org/) will be very beneficial before learning to use NgRx Store and the other state management libraries.

## Deeper Dive into the SHARI Principle

**S**hared: A good example of this concept would be authentication. An application that requires users to login will need to have a means of sharing the user's authentication state across all of its different components. This authentication state must be the same no matter where in the application it's being access. Therefore, having a central place to store the authentication and share that state with the entire application is critical. 

**H**ydrated: A good example of this concept would be Local Storage in the browser. Local Storage is a form of external storage that is located outside of an application itself. During the lifecycle of the application, information is given to and retrieved from Local Storage. For instance, when a user feeds the application information by filling out a form, that information is stored in the local storage. This is what is "hydration" means. The user "hydrates" the application with the information being stored. Then, when the user comes back to the application later, the application can pull that information back out of storage to use in the application again. This is the process of "re-hydration". When the user returns to the application, the application "rehydrates" on the information that was fed to it earlier.

**A**vailable: A good example of this concept would be Form Wizards. Form Wizards are a UX pattern designed to help users step through the process of feeding information to the application one step at a time. This pattern works by saving new information inputted by the users at each new step of the process without losing the information inputted at earlier steps. In other words, the information given by the user is _available_ at each step of the process. These types of forms would not work if the information were not able to be available at each step. Thus, information that a user is building up or giving to the application needs to be available no matter if a user navigates to a different page (i.e. a different route).

**R**etrieved: A good example of this concept would be Http Requests. A page that uses an Http request needs to retrieve certain information from a remote server and then populate the page or trigger a specific action based on the information retrieved. It would not be good functional programming practice to have the same function that makes the Http Request also do the work of performing the other actions needed by the page. Thus, we need some way to trigger other actions, also called _side effects_, based on some new pieces of information entering the application.

**I**mpacted: A good example of this concept would be pagination. Similar to the above concept, we often need to change some other kind of state based on changes to other parts of the application. For instance, when a user interacts with a paginated table, we are changing the view of the page based on interactions by the user with the application itself. We need some way to tell where that interaction comes from within the application, while at the same time change the view in the table. 

## What are some of the tradeoffs of ngRx?

For one, ngRx is an indirection framework. What do we mean by indirection? By indirection, we mean that ngRx separates out the business logic from the view layer. NgRx separates out this business logic into different files. When ngRx is used within an application, the program is "re-directed" to these other files to accomplish its goal. As mentioned, this separates out the concerns of the application. However, it also creates many more files within the application, making the application seem like it accomplishes certain tasks "indirectly".

This means that using ngRx means that you will have to navigate through many different files to trace the path of the business logic. This can make it difficult to trace the business logic. This can ultimately make your application more difficult to reason about. It is important to keep this tradeoff in mind. NgRx is not meant to simplify your application or make it easier to reason about.

However, the tradeoff here is that NgRx breaks business logic into smaller modules and methods that adhere to the "single responsibility priniciple". In other words, instead of trying to accomplish all of the logic in a central place, NgRx breaks different tasks into different modules that each only do one thing. This makes these modules easier to test and makes them much more explicit. Each part of ngRx does one specific thing. This is better functional programming practice, but it takes a whole-hearted embrace of this function programming paradigm. 

## Key Concepts

### Type Safety

Type safety is promoted throughout the architecture with reliance on the TypeScript compiler for program correctness. In addition to this, NgRx's strictness of type safety and the use of patterns lends itself well to the creation of higher quality code.

### Immutability and Performance

[Store](guide/store) is built on a single, immutable data structure which makes change detection a relatively straightforward task using the [`OnPush`](https://angular.io/api/core/ChangeDetectionStrategy#OnPush) strategy. NgRx Store also provides APIs for creating memoized selector functions that optimize retrieving data from your state.

### Encapsulation

Using NgRx [Effects](guide/effects) and [Store](guide/store), any interaction with external resources side effects such as network requests or web sockets, as well as any business logic, can be isolated from the UI. This isolation allows for more pure and simple components and upholds the single responsibility principle.

### Serializability

By normalizing state changes and passing them through observables, NgRx provides serializability and ensures the state is predictably stored. This allows the state to be saved to external storage such as `localStorage`.

This also allows the inspection, download, upload, and the dispatch of actions all from the [Store Devtools](guide/store-devtools).

### Testable

Because [Store](guide/store) uses pure functions for changing and selecting data from state, as well as the ability to isolate side effects from the UI, testing becomes very straightforward.
NgRx also provides test resources such as `provideMockStore` and `provideMockActions` for isolated tests and an overall better test experience.
