
# **Overview**

Mage is a one-click build tool for many of our technical requirements. Often, we end up re-solving problems which have been taken care of before, therefore wasting time on redundant jobs. Mage is built with the idea to decrease redundancy and improve product quality by extracting these common building blocks into "Spells" for being reused as a plug-and-play module.

# **Goals**

1. Build an extensible framework of modules that can be used to design scalable systems.

2. Deduplication of complex programs by providing an abstraction over complex code flows

3. Allow developers to focus on building business logic

4. Allow developers to create custom modules

5. Allow recursive development of modules (build modules over existing modules)

6. Utilise Battle tested and stable modules which are more reliable than components built from scratch.

# **Specifications**

Often, a developer faces the challenge of implementing methodologies widely available over the internet to suit their business use cases. This causes duplication of these complex architectures at various organizations, thereby reducing the efficiency of the overall development community.

## Use Cases

We face several use cases like this, a few of which have been mentioned below:

1. Create a CRUD application over predefined database schema

2. Create database caching strategies such as Cache Aside, Write through, Write Behind, etc.

3. Enable Audit Logging for API calls and database updates

4. Have multiple API calls in series/parallel and combine results from them

5. Create an In-memory queue for processing messages serially

6. Enable circuit breakers for dependencies

As we see, these use cases require redundant development from the developer which reduces his/her development efficiency and increases the time required to ship the product.

Mage is designed to be an open-sourced module-based development framework that is designed to create "generic" spells (or modules) for end customers to use, meanwhile keeping the code simple to understand.

Mage allows developers to define a configuration for the most complex and redundant part of the code using a UI utility which then, "magically" generates code according to the requirements. 

## Developer Concerns

**Concern**: Auto-generated code is generally hard to understand and review, which would greatly increase the review process.

**Solution**: Every Spell change would generate a configuration.yaml file which will be checked in along with the code. The reviewer could then review the configuration instead of the actual generated implementation, which would help him identify the changes without trying to understand the complex spell implementation, which is what the framework desires to achieve.

**Concern**: Mage would make my code tightly coupled to the framework.

**Solution**: Mage framework is completely open-source and fully abstracted into modules. The business logic developed can easily change the implementations for the interfaces with minimal changes in business logic, thereby making the business logic stay loosely coupled with Mage spells and their implementations.

**Concern**: Developers would still duplicate code since Mage might not be able to provide everything out of the box.

**Solution**: The idea with Mage will always be to simplify complex, redundant tasks. In case a problem statement fits out community standards and requirements and is generic enough, we would try to convert it into a problem statement that suits everyone’s needs and deliver modules with that. At the same time, the developers are free to add support for their custom modules by creating them by themselves and plugging them into Mage in a pre-defined mechanism.

**Concern**: What if Mage suddenly stops its support?

**Solution**: All our Mage codebases would be open-sourced and backed by an extensive community of developers who would love to make this development world a better place.

**Concern**: Will there be any proprietary modules?
**Solution**: Yes, there will be. Going forward, we will try to heavy-lift over our open source modules and create extensive modules to solve customer requirements. Meanwhile, we would try to provide solutions around cheaper deployment alternatives, monitoring, tracing, etc. which would make sense for small to medium size businesses.

**Concern**: We are a large organization and require Enterprise Support for complex software.

**Solution**: Mage provides extensive Enterprise support for all its proprietary as well as open-sourced modules backed by the organization. We are built with the sole idea of allowing developers to ship code faster and allow our extremely talented engineers to build confidence over the heavily lifted tasks. 

# **System Overview / Architecture**

The most elementary component of Mage is a **Spell**. A spell, as the word suggests, is designed to create components that magically create complex system architectures and portray them as elementary components.

The basic design for Mage UI would appear something like:

![System Design](/img/system/overall_architecture.png)

A typical user flow works as follows:

1. The user starts with a blank Mage UI which supports a Request Controller and a Logic Hub.

2. The user configures an endpoint in the request controller and defines a schema.

3. According to the request contract, an auto-generated code with a handler gets created. The user defines a custom logic, using this handler as the starting point.

 Now, while writing the business logic, the user identifies that he is required to fetch user details from an SQL table that has millions of user entries. The developer decides to have a Cache-Aside strategy for fetching user data where he fetches the user from a Redis cache and if unavailable, tries to fetch from the database.

1. The user then chooses the Data Spell and configures it to have a Cache-Aside strategy with SQL and Redis. 

2. For entity configuration (SQL host, Redis Host, ports, etc.), the user provides configs in a config.yaml file and uses their names to identify these values.

3. The user also defines its SQL schema and provides the fields which would be used to access data for this flow.

4. The user uses Mage’s **Cast** functionality to generate modules pertaining to these spells and accumulates them in an SDK especially catered to solve the requirement.

5. The user now connects to these implementations in his business logic, expecting the function to be working as expected in the documentation.

Why should you use Mage?

* Easy to bootstrap new services/APIs from scratch with minimal active development

* Incorporate several Maze spells using its enriched UI by adding new spells by just specifying block configurations.

* Open-Sourced spells are available for support by a large developer community making it highly reliable software.

* Easy to create custom spells and integrate with the platform, specific to your business requirements.

* Generate binaries that can be deployed anywhere. Mage also allows users to leverage state-of-the-art deployment platforms to manage deployments with integrated cloud platforms like AWS, GCP, Azure.

* Integrated support for logging, metrics, and tracing. You can focus on developing software to solve the Business problem and Mage takes care of providing tools to better debug your software.

* No tight coupling to the platform code gives the user the capability to move out of the platform easily.

## Technical Considerations

<table>
  <tr>
    <td>Spell Component</td>
    <td>SideCar</td>
    <td>Language-Specific </td>
  </tr>
  <tr>
    <td>API Spell ( Packed with Circuit Breaker, Retries )</td>
    <td>Adds sub-ms latency to the call.
No need to implement common libraries like retries, circuit breakers, etc in each language.
Only need to implement a thin wrapper in supported language to generate the </td>
    <td>We need to develop in every language and repeat the same in every language for all the changes slowing down the whole process.</td>
  </tr>
  <tr>
    <td>DB libraries</td>
    <td>Only need to benchmark and configure one sidecar compared to multiple libraries for multiple languages.

The additional latency in this scenario can be ignored compared to DB latency for most scenarios</td>
    <td>Hard to manage, changing libraries over time.</td>
  </tr>
</table>


