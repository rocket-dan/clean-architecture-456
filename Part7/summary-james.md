# Part.7 Architecture
## CH.25 Layers and Boundaries
Architectural boundaries are everywhere from simple to complex systems. To achieve clean architecture, manifesting boundaries between system components and defining layers between them is important.

In the chapter the author walks through an example of a simple game that has increasingly complex requirements and how that leads to the discovery of potential boundaries that aren't apparent.

Takeaways from this chapter were:
 - Even for simple software systems, we can come up with many architectural boundaries if we try hard enough.
 - There are inherent trade-offs between the cost of implementing boundaries VS the cost of ignoring them:
   * Implementing a boundary later on is costly and risky. Also, Permanently ignoring one where it is needed makes the project costly to maintain.
   * Implementing a strict boundary when it could be ignored is a waste in cost and leads to over-engineering.
 - So what we do is we weigh the cost of implementing vs ignoring to determine where boundaries should lie.
 - Note that this is not a one time decision and as the project evolves, we watch what things are causing friction in the development and maintainability and implement boundaries.
 - The goal is to implement the boundary at the point where the cost of implementation is less than the cost of ignoring.

 ## CH.26 The Main Component
The Main Component exists in every system and is responsible in creating, coordinating, and overseeing other parts of the system. The Main component is the ultimate detail and the lowest-level policy.

The job of the Main component is:
 - To create all factories, strategies and other global facilities, then hand over control to the high-level portions of the system.
 - To take in framework injected dependencies and distribute them and set them up throughout the system.

## CH.27 Services: Great and Small
The concept of vertical and horizontal layers and independence in service level architecture.

Recently, service-oriented "architectures" and microservices have become popular due to:
- Services seeming to be strongly decoupled from each other (only partially true).
- Services appearing to support independence of development and deployment (only partially true).

### Are Services an Architecture?
In short. No, services do not define the architecture because:

-> The architecture of a system is defined by boundaries that separate high-level policy from low-level detail. Services that simply separate application behaviors are little more than expensive function calls, and are not necessarily architecturally significant. Services are just function calls across processes and platform boundaries. 

![image](https://user-images.githubusercontent.com/22110122/141614019-7756e243-cea8-41b2-8112-af3fd5bb0309.png)

### Are the Benefits of Services Real?
#### The Decoupling Fallacy
One of the benefits of breaking a system up into services is that services are strongly decoupled from each other.
This is only partially true:
- Separate services are coupled by the structure of the data they pass to each other. If a field is added to the data in one place, all other services that get that data need to be modified
- Services might be using a shared resource (e.g redis) and a same data strucutre. If the schema of the data structure stored in the data store is modified all services that are dependent that data need to be adjusted.

#### The Independence of Development and Deployment Fallacy
The points above also show that independent development and deployment are also only partially true. For example, if services A and B share the database or the data format that is passed, when Service A changes the database schema, then Service B needs to change and the deployment needs to be coordinated.

### The Pitfall of Services by Functional Decomposition
A naive strategy for deciding how to divide a system into services is to divide them by functionality. For example, in a taxi-ordering system, the engineers could create the following microservices:
- Taxi Finder Service which keeps the inventory of all possible rides.
- Taxi Selector Service which selects the best ride for the user.
- Taxi Dispatcher Service which once selected, orders the ride.
The problem with type of decomposition is that it is a "purely horizontal" division and completely ignores the vertical layers.

The problem with this division is that a simple change of features would require a change throughout the whole system. For the example above, if the company now wanted the business to offer delivery of kities, the engineering team would be forced to modify every single microservice. Even worse, they face the risk of breaking the people delivery part of the business while working on the parcel delivery part.

![image](https://user-images.githubusercontent.com/22110122/141614338-61dc54a9-0b62-47ee-98ff-2144cdeb23ad.png)

### Objects To The Rescue
A solution to the problem is to divide services by vertical layers (Use Cases or groups of Use Cases) and have each service contain contain all functional concern it needs (all the horizontal layers).

The horizontal layers within each service are separated by architectural boundaries and follow the dependency rule.

Note that doing this requires the code to be divided vertically very early on. If you are in a situation where you already have microservices divided by functional decomposition, transforming the system to microservices by vertical decomposition might be impossibly expensive to do. However, not all hope is lost. If you already are in a situation where your system has microservices divided by functional decomposition, there are still things you can do to make it easier to change in the light of cross-cutting changes.
![image](https://user-images.githubusercontent.com/22110122/141614475-c9c54521-2384-4184-8add-e1ff981b7be1.png)
![image](https://user-images.githubusercontent.com/22110122/141614557-7c199b57-5233-43b2-aa01-26307117daa5.png)

## Ch.28 The Test Boundary
Tests are part of the architecture of a system. The good news is that from the point of view of the architecture, unit tests, integration tests, behaviour tests etc, all look the same. They all are detailed, low-level components that always depend inward to the code being tested. In a way, tests are the outermost circle of the clean architecture.

### Design for Testability
Testing should be part of the considerations when designing a system. Not considering tests as part of the design is a mistake that leads to fragile tests that are expensive to maintain and often end up getting discarded.

Fragile test problems occur when tests depend on volatile things (like the GUI) to test stable things. For example, writing GUI driven tests to test business rules can cause many use case tests to break when unrelated changes in the GUI happen.

To avoid these problems, design the system and the tests so that business rules can be tested without using the GUI (or other volatile things). The recommended way to do this is by creating a testing API that has "superpowers". These super powers include avoiding security constraints, bypassing expensive resources such as databases and allowing developers to force the system into particular testable states.

### The Pitfall of Structural Coupling
Having the test suite structurally map the production code (e.g. one test file for every class and tests for every public method) is problematic.

Structural coupling of tests makes the production code hard to change because the tests must change to follow the structure.

Over time, structural coupling limits the necessary independent evolution that tests and production code should undergo: Tests suites should naturally become more concrete and specific over time, whereas production code should naturally become more abstract and general.

## Ch.29 Clean Embedded Architecture
Firmware is software that is tightly coupled to the hardware it runs on. This coupling results in the need to re-write when the hardware changes.
Hardware changes at a very fast pace. To shield businesses from this, firmware engineers should be writing more software (code that has been isolated from the hardware it runs on) and less firmware.

### App-titude Test: The problem with just making it work
A general software good practice is:

1. “First make it work.” You are out of business if it doesn't work.
2. “Then make it right.” Refactor the code so that you and others can understand it and evolve it as needs change or are better understood.
3. “Then make it fast.” Refactor the code for “needed” performance.

Many engineers stop at "making it work" (aka the app-titude test) and never go beyond that.In embedded software in particular, often 1 and 3 are done together and 2 is never considered.

### The Target-Hardware Bottleneck Problem
Yes, embedded software has to run in special constrained hardware. However, embedded software development is not SO special; the principles of clean architecture still apply.

In embedded systems, you know you have problems when you can only test your code on the target hardware (as opposed to being able to test the business rules of your code independent of the hardware). This is known as the target hardware bottleneck problem. A clean embedded architecture is a testable embedded architecture.

### Conclusion
Letting all code become firmware is not good for your product’s long-term health. Being able to test only in the target hardware is not good for your product’s long-term health. A clean embedded architecture is good for your product’s long-term health.
