[![](https://img.shields.io/maven-central/v/org.scijava/pom-scijava-base.svg)](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.scijava%22%20AND%20a%3A%22pom-scijava-base%22)
[![](http://jenkins.imagej.net/job/pom-scijava-base/lastBuild/badge/icon)](http://jenkins.imagej.net/job/pom-scijava-base/)

The pom-scijava-base project is a Maven POM that serves as the base for all
Maven-based SciJava software, including:

| Fiji | ImageJ | ImgLib2 | KNIME | LOCI | SCIFIO | SciJava | SLIM Curve |
|:----:|:------:|:-------:|:-----:|:----:|:------:|:-------:|:----------:|
| [![Fiji](http://www.scijava.org/icons/fiji-icon-64.png)](https://github.com/fiji) | [![ImageJ](http://www.scijava.org/icons/imagej2-icon-64.png)](https://github.com/imagej) | [![ImgLib2](http://www.scijava.org/icons/imglib2-icon-64.png)](https://github.com/imglib) | [![KNIME](http://www.scijava.org/icons/knime-icon-64.png)](http://www.knime.org) | [![LOCI](http://www.scijava.org/icons/loci-icon-64.png)](https://github.com/uw-loci) | [![SCIFIO](http://www.scijava.org/icons/scifio-icon-64.png)](https://github.com/scifio) | [![SciJava](http://www.scijava.org/icons/scijava-icon-64.png)](https://github.com/scijava) | [![SLIM Curve](http://www.scijava.org/icons/slim-curve-icon-64.png)](https://github.com/slim-curve) |

## Extend pom-scijava, not pom-scijava-base!

It is encouraged that you _not_ extend this POM directly, and instead
use [pom-scijava](https://github.com/scijava/pom-scijava) as parent.
That way, your project will inherit the SciJava
[Bill of Materials](http://imagej.net/BOM), ensuring that
compatible versions of all dependencies are used.

For more information about Maven, see:

* [ImageJ Maven overview](http://imagej.net/Maven)
* [ImageJ Maven FAQ](http://imagej.net/Maven_-_Frequently_Asked_Questions)
