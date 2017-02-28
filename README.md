[![](https://img.shields.io/maven-central/v/org.scijava/pom-scijava-base.svg)](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.scijava%22%20AND%20a%3A%22pom-scijava-base%22)
[![](https://travis-ci.org/scijava/pom-scijava-base.svg?branch=master)](https://travis-ci.org/scijava/pom-scijava-base)

The pom-scijava-base project is a Maven POM that serves as the base for all
Maven-based SciJava software, including:

| Fiji | ImageJ | ImgLib2 | KNIME | LOCI | SCIFIO | SciJava | SLIM Curve |
|:----:|:------:|:-------:|:-----:|:----:|:------:|:-------:|:----------:|
| [![Fiji](http://www.scijava.org/icons/fiji-icon-64.png)](https://github.com/fiji) | [![ImageJ](http://www.scijava.org/icons/imagej2-icon-64.png)](https://github.com/imagej) | [![ImgLib2](http://www.scijava.org/icons/imglib2-icon-64.png)](https://github.com/imglib) | [![KNIME](http://www.scijava.org/icons/knime-icon-64.png)](http://www.knime.org) | [![LOCI](http://www.scijava.org/icons/loci-icon-64.png)](https://github.com/uw-loci) | [![SCIFIO](http://www.scijava.org/icons/scifio-icon-64.png)](https://github.com/scifio) | [![SciJava](http://www.scijava.org/icons/scijava-icon-64.png)](https://github.com/scijava) | [![SLIM Curve](http://www.scijava.org/icons/slim-curve-icon-64.png)](https://github.com/slim-curve) |

## pom-scijava-base vs. pom-scijava

It is encouraged that you _not_ extend this POM directly, and instead
use [pom-scijava](https://github.com/scijava/pom-scijava) as parent.

| pom-scijava-base | pom-scijava |
|:----------------:|:-----------:|
| "Low level" base POM, _without_ dependency version management. Extend pom-scijava-base only if you are a Maven expert, and have good reasons for doing so. | _Friendly_ base POM for SciJava software, _including_ dependency version management. Extend pom-scijava to inherit the unified SciJava [Bill of Materials](http://imagej.net/BOM): component versions which have been tested to work together. |

See these examples for guidance:

* [ImageJ1 plugin template](https://github.com/imagej/minimal-ij1-plugin)
* [ImageJ2 tutorials](http://github.com/imagej/imagej-tutorials)

## Getting help with Maven

For more information about Maven, see:

* [ImageJ Maven overview](http://imagej.net/Maven)
* [ImageJ Maven FAQ](http://imagej.net/Maven_-_Frequently_Asked_Questions)
