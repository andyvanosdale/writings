# Engineering Tenets

## Ask Questions
The fundamental aspect of engineering is asking questions. We use questions to challenge and validate assumptions and get answers to unsolved problems. They let us gain deeper insight to design better.

### The right questions are asked in the right way at the right time
It is not only enough to ask questions to other people to seek answers, but we must ask questions of ourselves, essentially an introspection. We must be constantly learning by first learning find the answer on our own. After asking questions to oneself that the answer hasn't been found through due dilligence, it is a great question to be posed to others.

Questions should not be asked to disparage others. Nor should questions put the onus on someone else to do the hard work of finding the answer for oneself.

### The 5 W's: Who, What, Where, When, and Why
These are the best starting point for asking questions. Ask these questions first and many more questions come after that.

### The 5 Why's
The **5 Why's** help to gain deeper insight into an issue. This is great for postmortems of issues. Once the first why is asked of the issue has been answered, use that answer to ask the next why and get that answered. Repeat the process until the insight has been fully understood.

For example. The issue is the service went down.

1. Why did the service go down? Because there a bug in processing the data.
2. Why was there a bug in processing the data? Because the data was malformed.
3. Why was the data malformed? Because the data wasn't sanitized at the source.
4. Why wasn't the data sanitized at the source? Because the data was user was trusted to input clean data.
5. Why was the user trusted to input clean data? Because there were instructions about how to input the data properly.

## Don't Give Up
Solving the complex problems we face everyday is tough. It's easy to give up. However, we must power through the urge to throw up our hands. We must continuously break down barriers and walls to find answers.

## Pair Programming
Working in a silo'd environment detaches team members from the team itself. Knowledge becomes specialized (and subsequently lost when a team member departs) and team cohesiveness degrades. Pair programming takes different forms, from multiple engineers at one terminal to screensharing to using IDEs that share a workspace. Working together creates a stronger bond throughout the team that lasts longer.

### Knowledge Sharing
By pairing, knowledge is shared by everyone involved. This reduces the bottleneck and funnel effect that having one person with the knowledge creates. When issues arise, the collective knowledge can work to tackle the problem and solve it quickly. However, it also allows for solo solving when other engineers are not around, such as when diagnosing an issue in the middle of the night.

### Learning by Doing
We learn better by actually doing. Our brains make better associations. While pair programming, everyone must be involved, and we all learn in different ways. We learn how others learn when we interact with our teammates on a more intimate level.

### Reduces Code Review Issues and Time
Through pair programming, others have had a chance to interact with the code, offer insight, and problem solve. The code is stronger as a result of having different minds working collectively. When the code is in review, the amount of issues raised and time spent is greatly reduced.

## Code Should Read Like A Novel
Code on its own should read like a novel. Through refactoring and naming, code will be able to be understand by any engineer that allows for easy changes, debugging, and handling support issues.

### Naming
With great naming comes great responsibility. Every modern day language allows for naming of various abstractions, such as variables, methods, and classes. Naming gives meaning and intention to the code. When reviewing stack traces, the names outputted allow for near immediate knowledge of not only where the problem is but why the problem is happening and the steps that led to the problem.

### Closures
Closures provide an easy way to encapsulate sub-logic within overall logic. However, as the sub-logic becomes more complex, the logic should be extracted to its own method. When view stack traces, it is easy to then pinpoint where the a problem originated through the method name, rather than a line of code in a closure.

### Refactoring
Methods should do one thing and one thing well. As code becomes more complex, breaking it down into more encapsualted functionality shows the intention behind that logic. Given a well-meaning and -intentioned name, the encapsulation can be better understood.

### More Lines of Code != More Complexity
There is a myth that more lines of code creates more complexity. However, it is the absense of encapsulation and proper logic that creates more complex code.

### Fail Fast
Code should be agile. There will be issues, so catch them early to have a greater likelyhood that the issue will be resolved. Create guard statements that fail fast in a method so that the actual business logic stands alone and is not encapsulated inside of nested statements. This makes the code more readable. Exceptions should be thrown at the source of an issue and not later on, as this will mask issues and make debugging more difficult.

## Logging
Logging is at the core of debugging an issue in the system. It is like telling a doctor about illness symptoms. The more verbose of information, the better a diagnosis can be made. Be verbose, but not overly verbose. Entry and exit from different processing layers, exceptions, non-exception failures are great logging use cases. Logging should occur as close to the source of a potential issue.

When logging, ensure that any correlation ID, request ID, and other non-PII/PCI identifying properties are logged to help understand data flows when support issues arise for specific events.

## Metrics and Monitoring
Metrics are critical to understanding how the system is performing. Use metrics to check that data is flowing and processing correctly. They should be used to know when services are starting up and shutting down as frequent restarts could indicate processing issues.

A lack of metrics is indicitive of a major failure in the system and should be remedied immediately. Heartbeats should be used to ensure the system is alive and responding. Polling intervals are a great way subsititute for a heartbeat.

## Documentation

### Code Comments
Code comments should explain why piece of code is doing what it is doing, if variable and method naming are not enough. Code comments must not explain what is happening. That is the job of proper variable, method, and class naming.

### READMEs
READMEs provide a great resource for documentation of the overall project (or sub-project). Sections explaining inputs into the project (such as environment variables), how to build and run tests, and an overall architecture description (with [ASCII diagrams](http://asciiflow.com/)) provide valuable documentation for new and existing members. READMEs should be a living document that is updated as code is updated.

## Testing

### Independent, Consistent, Reliable, and Predictable
Tests are run often. Each test should run on its own and not be dependent upon other tests. It must provide consistent results between subsequent runs and on different machines. They must be reliable and only break in the case of an actual failure. Finally, they must be predictable and not be beholden to environment changes, such as date and time.

### Unit Testing
Unit testing provides a unit-level documented contract. It gives great assurance that the code is working as advertised. However, the tests only test what it is coded to test. This is important to keep in mind and is why code coverage numbers are not an accurate representation of thoroughness of the unit testing.

#### Mock External Dependencies
External dependencies have their own issues. Through well-established contracts, the dependencies can be mocked out to provide targeted outputs and receive input that can be checked to ensure adherance to the contract.

#### Test Business Logic Through Publicly-Accessible Endpoints
Public endpoints are the established contract. The business logic encapsulated within those entrypoints must be tested through those entrypoints. Variables and methods that are marked as non-`public` (i.e.: `private`) are marked that way for a reason. Unit tests must not test those methods directly. Do not hack the objects to expose non-public data or functionality.

### Scenario Test with Integration Tests and Emulation
Integration tests provide a great way to test scenarios at a higher level. Each scenario should be its own integration test and be able to run independently of other scenarios. These should be automated and run in a CI/CD environment.

### End-to-End Test with Actual Dependencies
Lining up external dependencies requires much coordination and effort. End-to-end testing is a time to discover final issues in a controlled environment where all parties are involved. This is also the better area to test scenarios that have cascading effects. End-to-end tests can be either automated or manual.

## Code Reviewing
The engineer who created the code owns the code. Once checked in, the team owns the code. Code reviews are a last line of defense. The engineer should be confident that the code is in a state for a review. Through other means, such as pair programming, the engineer should

### Seek More than One Review As Necessary
As the engineer owning the code and the review, we must seek out any reviewers (think even stakeholders) as necessary. Through pair programming, this can be optimized as the reviewers may even have a better understanding of the code to ensure the review is smooth.

## Environment Deployable Everywhere
Code must be deployable to any environment in a predicable and repeatable fashion. The same code that runs in a production environment should be deployable to a local or a non-prod environment.

## Reduce Unnecessary Overhead
Overhead can greatly impact performance through CPU time and memory usage. We must use our due diligence early to catch these potential issues.

### The Right Dependencies
Identifying and using the right dependencies in the right manner is the critical first step to reduce unnecessary overhead. Dig into the dependencies' code. Ask questions. Seek time and space complexity.

### The Right Images
Use as slim of images as possible. Distroless base images do not contain shells, lowering the size and overhead of the image. It gives the security benefit of making it harder for anyone to inspect what is happening inside a container (outside of metrics or logging). This also prevents access to secrets that are pushed via environment variables.

## Secret Management
We must take great care and responsiblity to not allow rogue access to systems that we have permission to access. A secrets is anything that gives access to a secret resource. Typical examples are usernames, passwords, API keys, and private certificates.

### Versioning
Secrets must be versioned and deployed only through the normal change request protocol. The secrets themselves can be created through other means, such as through a console. Existing credentials must never be touched. There is no predictability when secrets are changed outside the versioning process and when code will access and use the new credentials. This allows for proper credential rotation; however, the called system must be setup to handle the rotation.

### Kubernetes
When using Kubernetes, use Kubernetes Secrets. Kubernetes Secrets are accessible only by team members with access to those secrets. Secrets can then be injected into containers through a Kubernetes Deployment securely.

### AWS
When using AWS (such as an EC2 instance or Lambda), use a combination of Identity and Access Management (IAM) roles, AWS Secrets Manager, and AWS Key Management System (KMS). IAM role assumption provides a great to lock down access to resources to only those who have access to that role. The AWS Secretes Manager provides storage for secrets and retrieval by AWS resources based upon role policies. The AWS KMS creates encryption keys that can be used to encrypt data, such as secrets stored by the AWS Secrets Manager.

### GitLab (Source Code)
When using GitLab to deploy, secrets for deployment must be stored in the GitLab CI/CD Secrets. These will be automatically injected into the jobs as environment variables. Care must be taken to not expose the credentials in the standard output or saved to any remote storage.

### NAuth/NIU
Use NAuth/NIU to get expiring session tokens to specific AWS IAM roles when deploying. This ensures that only our team has authorized access to the roles.

## Prod is Sacred
The production environment is sacred. While the engineering team is given great flexibility and access to production, the access should not be taken lightly. We must take great care of the production environment. Accidental changes to a production environment can be extremely detrimental to the business and erodes trust from customers. The [AWS Netflix Outage of Christmas Eve](https://aws.amazon.com/message/680587/) is a great example of why we establish this tenant.

### Changes to Prod only with CI/CD
Changes to the production environment must only go through CI/CD. This creates a ledger of changes made and allows for auditing. To prevent accidental deployment to a production environment, use explicit deployment roles in IAM and lockdown access to the roles. For example, the production deployment role should not be allowed to be assumed by the dev team's IAM role.
