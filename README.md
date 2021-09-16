# graydavid-parent

This project is a top-level parent pom for all projects in the `io.github.graydavid` groupId. It defines all common settings and expectations... except for style settings, which can be found in the `graydavid-style-parent` project. (This split is necessary, since `graydavid-style-parent` uses `graydavid-style` which uses the current parent project as a parent itself. So, we need the style-parent project to avoid circular dependencies.) So, most projects should inherit from `graydavid-style-parent` instead.

## Adding this project to your build

This project follows [semantic versioning](https://semver.org/). It's currently available via the Maven snapshot repository only as version 0.0.1-SNAPSHOT. The "0.0.1" part is because it's still in development, waiting for feedback or sufficient usage until its first official release. The "SNAPSHOT" part is because no one has requested a stable, non-SNAPSHOT development version. If you need a non-SNAPSHOT development version, feel free to reach out, and I can build this project into the Maven central repository.

## Contributions

I welcome any contributions! Here are some guidelines.

### Alignment

If your contribution is going to be big, let's talk things over first in order to align our thinking and avoid any potentially wasted effort and frustration. Feel free to open an issue under the relevant project. Provide details of what you want to change and why.

### Style

As many of the desired style checks have already been incorporated into each project's build.

#### Don't use "final" on method parameters and local variables

A more general policy on the "final" keyword, along with justification is [here](https://stackoverflow.com/a/154510). 

#### Don't use the "var" keyword at all, ever

I know this is extreme. I know there are (good) tips on when to use var [here](http://openjdk.java.net/projects/amber/LVTIstyle.html). I've worked on projects in the past that mandated the most important rule there ```Consider var when the initializer provides sufficient information to the reader.``` The problem (for me) is that people would try to push the boundary towards using var in more cases. I don't want to spend cycles arguing over the merits of using var in specific cases; it's just not worth it. Bite the bullet, write out the type, and let's move on :)

#### Keep methods small

A typical method should be no more than 10-15 lines long. There are trade-offs here and so there are exceptions. However, if you find yourself passing this limit, stop to ask why and whether you can refactor part of the code to a new method. The goal is that a reader should easily be able to step through a method and keep everything it's doing in their head. Refactoring to a new method adds an abstraction that (should) help with that. This goal is why some of the other rules exist (e.g. we don't need "final" on local variables if the method is small enough that reassigning them would ever be hidden from us).

#### Keep indentations small

There should be a maximum of 3 levels of indentation to any code block at all. Take this example:

```java
Integer a = 1; // Level one
if(a == 2) { // Level one
    for(int i = 0; i < 5; ++i){ // Level two
        try{ // Level three
          int j = 4; // Level four (don't do this)
          ....
        }
    }
}
```

Adding more levels of indentation means adding more branches that the user has to keep in mind at once. Instead consider taking one of those levels and refactoring it out to a separate method.

Note: consider lambdas as adding another level of indentation. For example:

```java
List<Integer> integers = ...; // Level one
integers.stream().forEach( i -> {System.out.println(i);} ); // Level one... and then level two
```

In this example, just using a forEach method call rather than a for-each loop doesn't change the (additional) level of indentation. 

#### Follow Effective Java

Follow all of [the advice](https://github.com/HugoMatilla/Effective-JAVA-Summary) from Effective Java... unless it's explicitly contradicted here.

#### <Placeholder>

These rules are a good start: they're not exhaustive. Expect it to be fluid/new rules to be added as discovered. If I add a new rule during a code review, it's only to help the next person; please believe that it's in good faith.

### Commits

#### Commit size

Commits should always be focused on a single issue. Please break-out any unrelated changes into a separate commit... unless the change is extremely small and doesn't distract from the main point of the review (e.g. maybe renaming a variable in a separate file is okay... but it's always up to the discretion of the reviewer of what is and what isn't "extremely small").

Commits should be small enough that the user can easily tell what's going on. Nobody's going to like a review that touches every file in the codebase with delicate/intricate changes. If you find yourself in that situation, break-up the commit into several commits and submit them for review separately. In the end, counterintuitively, this will save time for everybody.

#### Rebase commits

Rebase all of your local commits into a single commit before submitting a pull request for review. One pull request per review and vice-versa.

#### Commit message

All commit messages should follow this general format:

```
<Summary -- this is a short, less-than-one-sentence description of commit. This is the *what* of your changes.>
<Blank line -- remember to insert a blank line after the summary. This helps ensure separation in commands like "git history".>
* Motivation: <insert a more-detailed description of changes here. This should answer *why* you're making these changes.>
* Design: <this is where you answer *how* you've made the changes, including an explanation of how the pieces fit together and potentially alternative designs that you rejected.>
```

Including links is highly encouraged for any part if it provides more concrete details concisely. Still provide the gist of the changes in the commit message, though. Here's an example

```
Add a new type of String decorator

* Motivation: https://example.com/jira-0001 -- we need a new decorator to help provide more debug information in case of errors. This was a major pain point during the last production issue.
* Design: follow the existing decorator pattern, as established with Integers and Doubles.
```

#### Giving credit

I want to give you credit for your changes, but I'm new to github, and I'm not sure the best way to do that, yet. I only put my name in the copyright at the top of every file, because the license required something to be put there... well, and I also want credit for my work... but I also want to give you credit, too. I'm open to suggestions. 