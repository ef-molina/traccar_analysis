# JDepend Architectural Analysis

## Overview

This project includes JDepend for architectural analysis. JDepend traverses Java class files and generates design quality metrics for each Java package, helping to identify design problems and architectural issues.

## Running JDepend

To run JDepend and generate an architectural analysis report:

```bash
./gradlew jdependReport
```

## Report Location

After running the task, the XML report will be generated at:
```
build/reports/jdepend/jdepend-report.xml
```

## Metrics Provided

The JDepend report includes the following metrics for each package:

### Package Metrics

- **Total Classes**: Total number of classes in the package
- **Concrete Classes**: Number of concrete classes
- **Abstract Classes**: Number of abstract classes and interfaces

### Coupling Metrics

- **Ca (Afferent Coupling)**: Number of packages that depend on this package
- **Ce (Efferent Coupling)**: Number of packages this package depends on

### Quality Metrics

- **A (Abstractness)**: Ratio of abstract classes to total classes (0 to 1)
  - 0 = completely concrete
  - 1 = completely abstract
  
- **I (Instability)**: Ratio of Ce / (Ce + Ca) (0 to 1)
  - 0 = maximally stable
  - 1 = maximally instable
  
- **D (Distance from Main Sequence)**: Measure of balance between abstractness and stability
  - Calculated as: |A + I - 1|
  - Values close to 0 are ideal
  - Values close to 1 indicate potential design problems

### Package Dependencies

The report also shows:
- Dependencies between packages
- Dependency cycles (which should be avoided)
- Concrete and abstract classes in each package

## Interpreting Results

### Good Design Indicators

- Low Distance (D) values (close to 0)
- No dependency cycles between packages
- Stable packages (low I) should be abstract (high A)
- Unstable packages (high I) should be concrete (low A)

### Potential Issues

- High Distance (D) values suggest misplaced abstractions or concretions
- Dependency cycles indicate tight coupling
- Stable concrete packages are difficult to extend
- Unstable abstract packages may indicate over-engineering

## Integration with Build

JDepend runs independently and is not part of the standard `check` task. Run it manually when you want to perform architectural analysis:

```bash
# Run only JDepend analysis
./gradlew jdependReport

# Run full build without JDepend
./gradlew build
```

## Known Warnings

You may see warnings like "Unknown constant: 18" when analyzing code compiled with newer Java versions. These are informational warnings from JDepend about bytecode features it doesn't fully recognize, but the analysis still works correctly.

## Further Reading

- [JDepend Official Documentation](https://github.com/clarkware/jdepend)
- [Robert C. Martin's Stability Metrics](http://www.objectmentor.com/resources/articles/stability.pdf)
