# Holonic Graph Architecture (HGA)

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC_BY_4.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![W3C RDF 1.2](https://img.shields.io/badge/W3C-RDF_1.2-blue)](https://www.w3.org/TR/rdf12-concepts/)
[![SHACL 1.2](https://img.shields.io/badge/SHACL-1.2-blue)](https://www.w3.org/TR/shacl/)

**Namespace:** `https://ontologist.io/ns/holon#`  
**Preferred prefix:** `holon:`  
**Version:** 0.9.0  
**Author:** Kurt Cagle · [The Ontologist](https://ontologist.substack.com) · [The Inference Engineer](https://inferenceengineer.substack.com)

---

## Overview

The **Holonic Graph Architecture (HGA)** is a four-layer RDF 1.2 knowledge representation framework grounded in the W3C semantic web stack (OWL 2, SHACL 1.2, SPARQL 1.1, Turtle 1.2). It provides a principled model for organising knowledge as *holons* — units that are simultaneously self-contained wholes and constituent parts of larger structures — following Arthur Koestler's definition in *The Ghost in the Machine* (1967).

HGA integrates:
- **Formal ontology** (OWL 2 Full) for class hierarchy and property semantics
- **Constraint validation** (SHACL 1.2) for normative boundary enforcement
- **Active Inference dynamics** (Friston 2010–) mapped onto the static layer model
- **Narratological theory** (Genette 1980) for narrative and historiographic applications
- **DataBook pattern** for composable, distributable knowledge fragments

---

## The Four-Layer Model

Every holon in HGA is decomposed into four graph layers, each capturing a distinct epistemic perspective:

| Layer | Class | Role | Active Inference Analog |
|-------|-------|------|------------------------|
| L1 | `holon:SceneGraph` | Interior / immanent state | Internal states |
| L2 | `holon:DomainGraph` | Boundary / normative | Markov blanket |
| L3 | `holon:ContextGraph` | Relational / situational | Sensory/action states |
| L4 | `holon:HolonicGraph` | Meta-level / compositional | Generative model |

### Key Structural Properties

- **L2 ≅ Markov Blanket**: The DomainGraph separates internal (L1) states from external context (L3). SHACL constraint violations at L2 are interpreted as *prediction errors* — the gap between prior expectations encoded in the domain graph and observed data.
- **Spawn**: The `holon:spawn` operation (at L4) is the primary mechanism for dynamic sub-holon instantiation.
- **Portals**: Cross-holon communication is mediated exclusively through typed `holon:Portal` interfaces at the L2 boundary, preserving Markov blanket integrity.

---

## Namespaces

HGA uses a modular four-namespace system:

| Prefix | Namespace | Purpose |
|--------|-----------|---------|
| `holon:` | `https://ontologist.io/ns/holon#` | Core holonic architecture |
| `portal:` | `https://ontologist.io/ns/portal#` | Portal interface specialisations |
| `agent:` | `https://ontologist.io/ns/agent#` | Agent and actor specialisations |
| `session:` | `https://ontologist.io/ns/session#` | Session and temporal context |

### Relevant W3C Namespaces

| Prefix | Namespace | Role in HGA |
|--------|-----------|------------|
| `rdf:` | `http://www.w3.org/1999/02/22-rdf-syntax-ns#` | Core RDF primitives; RDF 1.2 triple terms |
| `rdfs:` | `http://www.w3.org/2000/01/rdf-schema#` | Class/property hierarchy |
| `owl:` | `http://www.w3.org/2002/07/owl#` | Ontology, FunctionalProperty, disjointness |
| `xsd:` | `http://www.w3.org/2001/XMLSchema#` | Datatype constraints |
| `sh:` | `http://www.w3.org/ns/shacl#` | SHACL 1.2 constraint shapes |
| `skos:` | `http://www.w3.org/2004/02/skos/core#` | Concept scheme and taxonomy |
| `dcterms:` | `http://purl.org/dc/terms/` | Ontology metadata |
| `prov:` | `http://www.w3.org/ns/prov#` | Provenance on reified triples |

---

## Repository Structure

```
holon-graph/
├── README.md                          # This file
│
├── ontology/
│   ├── holon-core.ttl                 # Core OWL 2 ontology (holon: namespace)
│   ├── portal-ontology.ttl            # Portal specialisations (portal: namespace) [planned]
│   ├── agent-ontology.ttl             # Agent/actor specialisations (agent: namespace) [planned]
│   └── session-ontology.ttl           # Session/temporal context (session: namespace) [planned]
│
├── shapes/
│   └── holon-shapes.ttl               # SHACL 1.2 normative constraint shapes
│
├── taxonomies/
│   └── holon-taxonomy.ttl             # SKOS concept scheme and taxonomy
│
├── examples/
│   ├── minimal-holon.trig             # Minimal valid holon instance [planned]
│   ├── actor-holon.trig               # ActorHolon with all four layers [planned]
│   └── dungeon-kestrel-keep.trig      # Full narrative world instance [planned]
│
├── queries/
│   └── holon-queries.sparql           # Reference SPARQL 1.1 queries [planned]
│
└── docs/
    ├── layer-model.md                 # Layer model specification [planned]
    ├── active-inference-mapping.md    # Active Inference integration [planned]
    ├── narrative-extension.md         # Narratological extension [planned]
    └── databook-pattern.md            # DataBook pattern specification [planned]
```

---

## Quick Start

### Minimal Holon Instance (Turtle 1.2)

```turtle
@prefix holon: <https://ontologist.io/ns/holon#> .
@prefix xsd:   <http://www.w3.org/2001/XMLSchema#> .
@prefix rdfs:  <http://www.w3.org/2000/01/rdf-schema#> .

:myHolon
    a holon:Holon ;
    rdfs:label "My First Holon"@en ;
    holon:holonID "my-holon-001" ;
    holon:createdAt "2026-04-02T09:00:00Z"^^xsd:dateTime ;
    holon:hasSceneGraph   :myHolon-scene ;
    holon:hasDomainGraph  :myHolon-domain ;
    holon:hasContextGraph :myHolon-context .

:myHolon-scene   a holon:SceneGraph .
:myHolon-domain  a holon:DomainGraph .
:myHolon-context a holon:ContextGraph .
```

### SHACL Validation (Apache Jena)

```bash
shacl validate \
  --shapes shapes/holon-shapes.ttl \
  --data examples/minimal-holon.trig
```

### SHACL Validation (Node.js / rdf-validate-shacl)

```javascript
import factory from 'rdf-ext';
import SHACLValidator from 'rdf-validate-shacl';
import { fromFile } from 'rdf-utils-fs';

const shapes = await fromFile('shapes/holon-shapes.ttl');
const data   = await fromFile('examples/minimal-holon.trig');
const validator = new SHACLValidator(shapes, { factory });
const report = await validator.validate(data);

console.log('Conforms:', report.conforms);
for (const result of report.results) {
  console.log(result.severity, result.message, result.focusNode.value);
}
```

---

## Theoretical Foundations

### Koestler — Holons and Holarchies

Arthur Koestler introduced the concept of the *holon* in *The Ghost in the Machine* (1967) to resolve the part/whole problem in biological and social systems. Every stable structure in nature is simultaneously a whole (with respect to its constituents) and a part (with respect to the system it belongs to). HGA applies this insight to knowledge representation.

### Friston — Active Inference

Karl Friston's Active Inference framework (2010–) models agents as systems that minimise variational free energy by updating generative models and acting on the environment. HGA maps this dynamic process onto the static layer structure:

- **L2 DomainGraph ≅ Markov blanket**: enforces the conditional independence that makes agency possible
- **SHACL violations ≅ prediction error**: constraint failures signal model-environment mismatch requiring revision
- **holon:spawn ≅ model elaboration**: creating new sub-holons is how a holon extends its generative model

### Genette — Narratological Extension

Gérard Genette's narratological triad (*Narrative Discourse*, 1980) — fabula, sjuzhet, narration — maps cleanly onto the four holonic layers:

| Genette | HGA Layer | Description |
|---------|-----------|-------------|
| Fabula | L1 SceneGraph | The events as they occurred in the story-world |
| Story-world rules | L2 DomainGraph | What is possible/permitted in the world |
| Sjuzhet | L3 ContextGraph | How events are ordered and presented |
| Narration | L4 HolonicGraph | The act and perspective of telling |

---

## Related Ontologies

| Ontology | Prefix | Relationship |
|----------|--------|-------------|
| [wardley-graphs](https://github.com/kurtcagle/wardley_graphs) | `wardley:` | `wardley:WardleyMap rdfs:subClassOf holon:ProjectionGraph` |
| [W3C Context Graph CG](https://w3c.github.io/context-graph/) | `cga:` | Parallel specification; HGA is the implementation ontology |
| [shaclConverter](https://github.com/kurtcagle/shaclConverter) | — | SHACL transformation toolkit for HGA shapes |

---

## Status and Roadmap

**Current version: 0.9.0 (Working Draft)**

The core OWL ontology and SHACL shapes are stable for reference. The following are planned for subsequent releases:

- [ ] `portal-ontology.ttl` — portal specialisations and typed channels
- [ ] `agent-ontology.ttl` — agent lifecycle, goal structures, belief states
- [ ] `session-ontology.ttl` — temporal session management, late binding deferral
- [ ] Example TriG instances (minimal, actor, full narrative world)
- [ ] SPARQL 1.1 reference query library
- [ ] DataBook pattern specification and tooling
- [ ] Active Inference integration documentation

---

## License

This ontology is published under [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/).

---

## Author and Citation

**Kurt Cagle**  
Consulting Ontologist · IEEE Standards Editor (IEEE Spatial Web Foundation) · W3C Context Graph Community Group

- [The Ontologist](https://ontologist.substack.com) (Substack)
- [The Inference Engineer](https://inferenceengineer.substack.com) (Substack)
- [The Cagle Report](https://www.linkedin.com/newsletters/the-cagle-report-6672594836610252800/) (LinkedIn)

To cite this ontology:

```
Cagle, K. (2026). Holonic Graph Architecture Core Ontology (v0.9.0).
https://ontologist.io/ns/holon · https://github.com/kurtcagle/holon-graph
```

---

*Copyright 2026 Kurt Cagle. Licensed under CC BY 4.0.*
