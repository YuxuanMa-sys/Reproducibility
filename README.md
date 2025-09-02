# CS527 Reproducibility Research Project

## Overview

This repository contains a comprehensive reproducibility study conducted as part of CS527 (Reproducibility in Computer Science) at the University of Illinois. The project focuses on reproducing research papers and evaluating their computational reproducibility using Docker containers.

## Project Structure

### Research Papers
The project contains 25+ research papers, each with:
- **YAML configuration files** (e.g., `0001ST18.yaml`, `BegoliCHML18.yaml`) - containing metadata and build instructions
- **Docker directories** (e.g., `0001ST18/`, `BegoliCHML18/`) - containing Dockerfiles for containerization
- **Build status tracking** - documented in progress files

### Key Files

#### Configuration Files
- `*.yaml` - Research paper metadata and build configurations
- `mp1.yaml`, `mp2_input.yaml` - Machine problem configurations

#### Progress Documentation
- `progress1.md` through `progress7.md` - Weekly progress reports
- `cumulative-progress.md` - Overall project status
- `course_grade.txt` - Final course grades

#### Course Materials
- `mp0.md`, `mp1.yaml`, `mp2.md` - Machine problem descriptions
- `draft-proposal.md`, `final-proposal.md` - Project proposals
- `your_flaky_tests.csv` - Flaky test analysis data

## Research Papers Included

### Successfully Reproduced (BuildAndTestSuccess)

- **0001ST18** - OCaml-based constraint solving system (CoAR) for predicate constraint satisfiability checking using SMT solvers
- **BegoliCHML18** - Apache Calcite build system for data management and query optimization framework
- **BornholtT18** - Program synthesis using Rosette framework for automated code generation from specifications
- **CaiGZACOTTW18** - GAM (Global Address Space Memory) system for distributed memory management and high-performance computing
- **ChenZW24** - Recent research paper on distributed systems and data processing
- **ConnellFSDP24** - Distributed systems research focusing on fault tolerance and scalability
- **FengKRR12** - Database systems research on query optimization and performance
- **GehrMTVWV18** - Neural network verification tool for analyzing deep learning model robustness
- **GodboleCMS24** - Machine learning framework for computational modeling and simulation
- **KangKSLPSKCCHY18** - Program analysis tool for static code analysis and optimization
- **LimFAK11** - Google Test framework implementation for C++ unit testing and test automation
- **PaponCZA24** - Recent research on computational algorithms and data structures
- **PrakriyaCBSC24** - Systems research on computer architecture and performance optimization
- **PulteFDFSS18** - Compiler research focusing on code generation and optimization techniques
- **ShahKZ17** - Database systems research on transaction processing and concurrency control
- **WeiDR18** - Functional programming research on abstract machines and definitional interpreters
- **WeiZZCMLFC23** - Machine learning framework for federated learning and collaborative training
- **YangLW12** - Bioinformatics algorithms (ALAE) for local alignment with affine gap penalties in sequence databases

### Partially Successful

- **AhnHKJ24** - Kernel-level performance monitoring tool (bperf) for blocked samples analysis; TestFail due to permission issues with performance event access
- **ChenYZZJZ24** - Spectrum database system for high-performance data processing; BuildCrash due to dependency configuration issues with chfast/intx

### Failed Reproductions

- **KaiserZKRD18** - Mtac2 meta-programming framework for Coq theorem prover; BuildCrash due to OPAM repository access issues and SSL certificate problems
- **LeeHJLRL18** - Clang-twin compiler tool for program analysis; BuildIncomplete due to excessive build time (5-6 hours) and memory constraints
- **MbakoyiannisTK18** - PCIe FPGA acceleration framework for hardware acceleration; BuildIncomplete due to proprietary Vivado software requirements
- **JinLJMSHZ18** - Caffe deep learning framework for neural network training; BuildCrash due to Python package dependency resolution failures

## Methodology

### Docker-Based Reproducibility
Each research paper is containerized using Docker with:
1. **Base images** - Ubuntu, OCaml/OPAM, or other specialized images
2. **Dependencies** - System packages, language-specific tools
3. **Build process** - Compilation and setup instructions
4. **Test execution** - Validation of functionality

### YAML Configuration Schema
The project uses a structured YAML format for each paper:
```yaml
dblp_url: [paper URL]
computational_status:
  type: ComputationalArticle
  tool_names: [list of tools used]
  source_search:
    type: SourceFound
    build_attempt:
      base_image: [Docker base image]
      build_directives: [build steps]
      test_directives: [test steps]
      result: [BuildAndTestSuccess/BuildCrash/etc.]
```

## Key Findings

### Reproducibility Statistics
- **Total Papers**: 25+
- **Successfully Reproduced**: ~18 papers (72%)
- **Partially Successful**: ~3 papers (12%)
- **Failed Reproductions**: ~4 papers (16%)

### Common Issues Encountered
1. **Missing dependencies** - Outdated package versions
2. **Build environment differences** - OS/compiler version mismatches
3. **Test failures** - Flaky tests or missing test data
4. **Documentation gaps** - Incomplete build instructions

### Technical Challenges
- **Base image compatibility** - Some papers require specific OS versions
- **Shell command parsing** - Complex build scripts
- **Network dependencies** - External repositories and packages
- **Test data availability** - Missing or large test datasets

## Usage

### Running Individual Papers
```bash
# Navigate to a specific paper directory
cd 0001ST18/

# Build the Docker container
docker build -t paper-0001ST18 .

# Run the container
docker run paper-0001ST18
```

### Validating YAML Configurations
The project includes a YAML validator that converts configurations to Dockerfiles:
```bash
# Example validation (requires project-specific tools)
validate_yaml 0001ST18.yaml
```

## Technical Requirements

### System Requirements
- **Docker** - For containerization
- **Git** - For version control
- **Python** - For validation scripts (if applicable)

### Base Images Used
- `ubuntu:20.04` - Standard Ubuntu
- `ocaml/opam:ubuntu-22.04-ocaml-5.1` - OCaml development
- Various language-specific images

## Contributing

This is a course project repository. For reproducibility research contributions, please refer to the original research papers and their associated repositories.

## License

This project is part of academic coursework. Please respect the licenses of the original research papers and software included in this study.

## Acknowledgments

- **Course Instructor**: Sam (CS527)
- **Research Community**: Authors of reproduced papers
- **Open Source Community**: Docker, Ubuntu, and other tools used

---

*This README documents a comprehensive reproducibility study conducted as part of CS527 coursework at the University of Illinois. The project demonstrates the challenges and successes of reproducing computational research in controlled environments.*
