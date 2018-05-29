
# Company Development Standards - Guiding Principles

The goal of Company's Guiding Principles for software development is to provide principles and patterns of software architecture to be followed by developers across different projects and languages. It is like a style guide but for software patterns and practices rather than specific languages; it is more philosophical and abstract in nature.

### Vocabulary

Defining a common vocabulary is necessary to facilitate clear and useful communication among developers and stakeholders.

* **Software Architecture** refers to the organization and structure of source code: classes, functions, variables, naming conventions, etc.
* **Application Architecture** refers to the collection of systems necessary to run the entire application, e.g. MySql, Redis, php, external dependencies, etc.

Company's Guiding Principles refer primarily to **Software Architecture**.

## Guiding Principles

### Object Oriented Programming

Most of Company's guidelines naturally flow into or out of the concept of Object Oriented Programming (OOP). OOP separates blocks of code into self-contained objects which are designed to accomplish specific goals. This modularity achieves a number of principles.

* **Separation of Concerns**<sup>[1](#source2)</sup>. Divide your application into distinct features with as little overlap in functionality as possible. OOP makes this simple, as each class and function is designed for a specific purpose.
* **Single Responsibility principle**<sup>[1](#source2)</sup>. Each component or module should be responsible for only a specific feature or functionality, or aggregation of cohesive functionality. This is similar to the concept of Separation of Concerns and is made easy with OOP.
* **Principle of Least Knowledge**<sup>[1](#source2)</sup> (also known as the Law of Demeter or LoD). A component or object should not need to know about internal details of other components or objects.
* **Don’t repeat yourself (DRY)**<sup>[1](#source2)</sup>. You should only need to specify intent in one place. For example, in terms of application design, specific functionality should be implemented in only one component; the functionality should not be duplicated in any other component. OOP provides for this through the use of inheritance.
* **Clean Code**<sup>[2](#source1)</sup>. Most software defects are introduced when changing existing code. The reason behind this is that the developer changing the code cannot fully grasp the effects of the changes made. Clean code minimizes the risk of introducing defects by making the code as easy to understand as possible.
* **Naming**. Names of classes, functions, variables, fields, properties should reflect what the entity is or does. They should be descriptive and precise. Long names are preferred, if necessary, to vague or cryptic names.
    * This may seem obvious, but when naming anything that will be visible in client side source code, avoid using names including strings like `worm`, `virus`, `trojan`, etc.
* **Loose Coupling**. Applications should be internally structured in a loosely-coupled fashion. That is, it should be simple to swap in and out various application components (usually at write-time, but sometimes at run-time as well). It is typical to leverage Dependency Injection to implement loose coupling, which is usually facilitated through an [application framework](#frameworks). Loose coupling enables, amongst other things, straightforward unit testing and lower cognitive overhead for making changes to a large codebase.
* **Class Design**. Classes should be small and purposeful. Ensure that methods, fields, and properties are made public only when necessary. Ideally, all public exposures of a class would be expressed in an interface or prototype implemented by the concrete class.
* **Methods**. Methods within classes should be short and purposeful. A good method accepts as parameters its required input, returns it's desired output, and minimizes side-effects (such as mutating object internal state). The ideal amount of side effects for a class method is zero, but in practice this requires a lot of forethought given to immutability and isn't always practical.
* **Configuration**. Configuration details should be stored in the environment. Company does this by having env files for each environment which are renamed when Jenkins builds in each environment.
    >A litmus test for whether an app has all config correctly factored out of the code is whether the codebase could be made open source at any moment, without compromising any credentials.<sup>[3](#source3)</sup>

### Common Components

For each language, there has naturally emerged a set of components or tooling that come to solve common problem(s) across projects. Company's adoption of such components is expressed per-language in the [Style Guide](style_guide.md) where appropriate under the subheading **Common Components**. These are official recommendations from the Standards Committee, and while they are not impossible to be amended they should be treated as authoritative.

- Individual exceptions can be made from common components under truly exceptional circumstances. For example, for some reason if PHP code needs to be deployed to an uncontrolled destination where the available PHP version is below the requirement for Lumen.
- To amend or add components, the typical [change request](README.md#submitting-change-requestsproposals) process must be followed. A strong case is required to amend these recommendations, so be prepared to defend your proposal.

### Frameworks

Frameworks are typically a [common component](#common-components), but they bear a little extra discussion in the greater context of guiding principles.

* Frameworks improve efficiency, productivity and reliability by providing easy to use abstractions or facades to work with common application infrastructure such as database connections and file handlers. This allows developers to focus on the unique requirements of the application at hand, rather than working with the "plumbing".
* Frameworks like Laravel and Angular2 use and encourage the use of OOP. Take the time to read their documentation and learn and use the benefits provided by them.
* Frameworks are easily extensible through the use of third party packages. This allows developers to use a robust, proven solution for a given problem rather than coding it themselves.

### Resources

Developers are encouraged to study the following resources for more in-depth knowledge on software design principles:

1. <a name="source2"></a>[MSDN Key Principles of Software Architecture](https://msdn.microsoft.com/en-us/library/ee658124.aspx)
1. <a name="source1"></a>[Clean Code](http://a.co/emX2190) by Robert C. Martin (here is a [Clean Code cheat sheet](http://www.planetgeek.ch/wp-content/uploads/2013/06/Clean-Code-V2.1.pdf))
1. <a name="source3"></a>[The Twelve Factor App](https://12factor.net/)
1. [Design Patterns](https://sourcemaking.com/design_patterns) (sourcemaking.com)
1. [Laracasts: Object Oriented Bootcamp](https://laracasts.com/series/object-oriented-bootcamp-in-php)

### Testing

Testing frameworks and standards are still being determined.

### Development Environments

Developers are free to use the IDE/editor of their choice for software development, however it is strongly encouraged to use an environment which has inbuilt functionality or available plugins for the following:

* Code Linting
* Style Checking or autoformatting
* Unit Testing

### Git Workflow

Company follows the [gitflow](http://nvie.com/posts/a-successful-git-branching-model/) workflow process for Git, originally described by Vincent Driessen. Using Git makes continuous integration easy and the gitflow workflow allows for easy integration of parallel development.
