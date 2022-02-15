[![](https://img.shields.io/maven-central/v/org.scijava/pom-scijava-base.svg)](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.scijava%22%20AND%20a%3A%22pom-scijava-base%22)
[![](https://github.com/scijava/pom-scijava-base/actions/workflows/build-main.yml/badge.svg)](https://github.com/scijava/pom-scijava-base/actions/workflows/build-main.yml)

The pom-scijava-base project is a Maven POM that serves as the base for all
Maven-based SciJava software, including:

| Fiji | ImageJ2 | ImgLib2 | KNIME | LOCI | SCIFIO | SciJava | FLIMLib | Virtual Cell |
|:----:|:-------:|:-------:|:-----:|:----:|:------:|:-------:|:----------:|:------------:|
| [![Fiji](https://scijava.org/icons/fiji-icon-64.png)](https://github.com/fiji) | [![ImageJ2](https://scijava.org/icons/imagej2-icon-64.png)](https://github.com/imagej) | [![ImgLib2](https://scijava.org/icons/imglib2-icon-64.png)](https://github.com/imglib) | [![KNIME](https://scijava.org/icons/knime-icon-64.png)](https://knime.org) | [![LOCI](https://scijava.org/icons/loci-icon-64.png)](https://github.com/uw-loci) | [![SCIFIO](https://scijava.org/icons/scifio-icon-64.png)](https://github.com/scifio) | [![SciJava](https://scijava.org/icons/scijava-icon-64.png)](https://github.com/scijava) | [![FLIMLib](https://scijava.org/icons/flimlib-icon-64.png)](https://github.com/flimlib) | [![Virtual Cell](https://scijava.org/icons/vcell-icon-64.png)](https://github.com/virtualcell) |

## pom-scijava-base vs. pom-scijava

It is encouraged that you _not_ extend this POM directly, and instead
use [pom-scijava](https://github.com/scijava/pom-scijava) as parent.

| pom-scijava-base | pom-scijava |
|:----------------:|:-----------:|
| "Low level" base POM, _without_ dependency version management. Extend pom-scijava-base only if you are a Maven expert, and have good reasons for doing so. | _Friendly_ base POM for SciJava software, _including_ dependency version management. Extend pom-scijava to inherit the unified SciJava [Bill of Materials](https://imagej.net/BOM): component versions which have been tested to work together. |

See these examples for guidance:

* [ImageJ2 command template](https://github.com/imagej/example-imagej-command)
* [ImageJ 1.x plugin template](https://github.com/imagej/example-legacy-plugin)
* [ImageJ2 tutorials](https://github.com/imagej/tutorials)

## Enforcer rules declared in this parent

The `pom-scijava-base` parent POM declares several
[enforcer](https://maven.apache.org/enforcer/maven-enforcer-plugin/) rules which
we believe make SciJava-based projects more reproducible and more consistent:

* __Plugin versions.__ Out of the box, Maven does not require plugins used to
  declare a version. But plugin versions must be declared to ensure
  reproducible builds. Otherwise, the version of Maven core you use at build
  time will determine which plugin versions are used, and the behavior might
  differ between builds.

* __No duplicate classes.__ If two dependencies define the same class, then
  it introduces the possibility of serious class-loading issues. Which version
  of the class should be chosen? In some scenarios, classes from one part of a
  certain library may be loaded from dependency `foo`, but classes from a
  different part of that same library at a _different version_ may be loaded
  from dependency `bar`. When this happens, difficult-to-understand compiler
  errors, or even runtime errors, may occur. Best practice is to ensure that
  all classes come onto the classpath from exactly _one_ source.

* __No too-new dependencies.__ When a project is compiled for Java version X,
  then it may not use any dependencies which require a version newer than X.
  Otherwise, your project is lying about needing only version X.

* __No circular dependencies.__ Actually, this is a central rule of Maven. But
  we configure the Enforcer to explicitly check for it, just to be safe. It is
  always possible to avoid circular dependencies; if you feel like you need
  one, you should instead solve it in one of the following ways, depending on
  how much code is co-dependent:
    1. Combine the co-dependent artifacts into a single artifact.
    2. Introduce a third artifact to house the co-dependent code, which depends
       on the other two artifacts.
    3. If all else fails, write to the
       [SciJava list](https://groups.google.com/group/scijava) for help.

* __Reproducible builds.__ This rule means no `SNAPSHOT` dependencies, no
  `SNAPSHOT` parents, and no `SNAPSHOT` plugin versions. A snapshot version is
  not immutable, which means that code which depends on a snapshot may build
  today, but not build tomorrow, if the snapshot is later changed. The best way
  to avoid this conundrum is to _never depend on `SNAPSHOT` versions_. Snapshot
  are best used for testing only; they can be used transiently, but their use
  should never make it onto the main integration branch
  (e.g., `main` or `master`) of a project. See also
  [Using snapshot couplings during development](https://imagej.net/develop/architecture#using-snapshot-couplings-during-development).

* __Developer roles.__ SciJava-based projects define developers and
  contributors with roles matching the
  [SciJava team roles](https://imagej.net/Team). Doing this is vital for
  consistency, and for communicating expectations to the community. By being
  careful about which developers are pledging which sorts of responsibility,
  the social status of each project becomes much clearer, and which social
  actions to take in various circumstances becomes a more tractable problem.
  We have
  [automated tooling](https://github.com/imagej/imagej.github.io/blob/main/assets/js/maven.js)
  which can populates a statbox sidebar for any component built on SciJava,
  including all components of the
  [ImageJ2 software stack](https://imagej.net/develop/architecture#definitions);
  this tooling requires SciJava developer roles to be present for sensible
  results.

* __Required metadata.__ Every SciJava-based project must override key pieces
  of metadata, including the `name`, `description`, `url`, `inceptionYear`,
  `organization`, `licenses`, `developers`, `contributors`, `mailingLists`,
  `scm`, `issueManagement` and `ciManagement` elements, as well as the
  `license.licenseName` and `license.copyrightOwners` properties.
  There are several reasons for requiring these overrides:
    * __Avoid inadvertent inheritance.__ The `pom-scijava-base` POM itself
      declares all of this metadata for itself (e.g., its `<scm>` block defines
      where the `pom-scijava-base` source code is managed, and this information
      is necessary for the tooling which cuts releases of the
      `pom-scijava-base` POM itself). For better and worse, when extending
      `pom-scijava-base`, the child POM inherits all of these elements (except
      for `<name>`, but that is the sole exception in the above list). If the
      child POM does not override each and every one of these elements, then it
      will inadvertently inherit the incorrect values from the
      `pom-scijava-base` parent. Furthermore, due to a quirk/limitation in
      Maven, if you specify an empty block (e.g., `<contributors />`), then the
      non-empty value from the ancestor will take precedence in the
      interpolated POM. Hence, we enforce that all of these fields are
      overridden with _non-empty_ values. See below for advice on how to best
      override specific metadata fields.
    * __Present the project's metadata simply and clearly.__ For humans, being
      able to look at a project POM and clearly see the metadata is very
      helpful for understanding the project. Whereas when inheritance is
      involved, the human must be patient enough to dig through the ancestor
      POMs manually looking for the information, or else knowledgeable enough
      to know that they should actually use `mvn help:effective-pom` and check
      the metadata there instead, to know the actual values.
    * __Make the metadata easier for tooling to consume.__ Maven-based tooling
      which uses the interpolated POM will be able to extract the correct
      inherited metadata from a POM, sure. But in many cases, it is much
      simpler and more natural, especially for Maven non-experts, to code
      tooling using shell scripts and similar approaches. In those cases, it is
      much easier if the tooling can simply extract the metadata from the child
      POM itself and be guaranteed that the values there are the correct ones,
      without needing to recurse into parent POMs whose contents may be less
      trivial to inspect.
    * __Make it easier to maintain license headers in the sources.__ Putting a
      license blurb at the top of each source file is legal best practice, but
      it is undeniably a hassle, which is one reason many projects do not
      bother. But with the `license-maven-plugin`, generating and maintaining
      these license headers becomes very easy—as long as the `inceptionYear`,
      `license.projectLicense` and `license.copyrightOwners` values are
      provided in the POM. At that point, you can just invoke `mvn
      license:update-file-header license:update-project-license` and your work
      is done.
    * __Encourage responsible metadata curation.__ As a project maintainer,
      _you_ are responsible for your project's metadata. Yes, it is a hassle to
      specify it. But regardless, you need to understand where (if anywhere)
      your project lives in SCM, which (if any) system is being used to
      automatically build it, and so on. You have legal and social obligations
      to clearly communicate the project license, to clearly document community
      expectations, to give credit where credit is due, etc.

The full set of Enforcer rules as of `pom-scijava-base` version 14.0.0 can be
[seen here](https://github.com/scijava/pom-scijava-base/blob/pom-scijava-base-14.0.0/pom.xml#L831-L926).

### How to override a field with an "empty" value

For some projects, you may have "empty" metadata fields, and you may be unsure
how best to override those values accordingly. The most common scenarios are:

*   If your project has no contributors, write:
    ```xml
    <contributors>
      <!--
      NB: Need at least one element to override the parent.
      See: https://issues.apache.org/jira/browse/MNG-5220
      -->
      <contributor>
        <name>None</name>
      </contributor>
    </contributors>
    ```
*   If your project has no discussion forum or mailing list, write:
    ```xml
    <mailingLists>
      <mailingList>
        <name>None</name>
      </mailingList>
    </mailingLists>
    ```
    But you are warmly welcome to use the
    [Image.sc Forum](https://forum.image.sc/) for discussing your project,
    so instead it is better to write:
    ```xml
    <mailingLists>
      <mailingList>
        <name>Image.sc Forum</name>
        <archive>https://forum.image.sc/tag/your-tag</archive>
      </mailingList>
    </mailingLists>
    ```
    Where `your-tag` is the tag you want to
    be used when discussing your project.
*   If your project has no CI, write:
    ```xml
    <ciManagement>
      <system>None</system>
    </ciManagement>
    ```
*   If your project has no issue tracker, write:
    ```xml
    <issueManagement>
      <system>None</system>
    </issueManagement>
    ```
*   If your project does not live in SCM, then write:
    ```xml
    <scm>
      <system>None</system>
      <url>None</url>
    </scm>
    ```
    But as an aside, in this case, we strongly encourage you to adopt an SCM;
    [check yourself before you wreck yourself](https://imagej.net/contribute/distributing).

## Getting help with Maven

For more information about Maven, see:

* [ImageJ Maven overview](https://imagej.net/develop/maven)
* [ImageJ Maven FAQ](https://imagej.net/develop/maven-faq)
