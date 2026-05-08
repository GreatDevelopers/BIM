# Introduction to OpenBIM: Creating Your First IFC Model

Welcome to the world of Digital Construction! As Civil Engineering students, you are entering an era where buildings are designed and managed digitally. This tutorial explains how to use Python to create an **IFC** (Industry Foundation Classes) file, which is the universal language for 3D building models.

To make this easier to understand, think of an IFC file like a digital blueprint that stores not just the geometry, but also the "meaning" of a building.

---

## 1. Setting Up the Environment
Before we build, we need our toolkit. In Python, we use "libraries" to perform specific tasks.

Imagine a construction project manager coordinating different specialists on a site.

- If there is a structural issue, the manager calls a structural engineer.
- If the air-conditioning system needs attention, an HVAC technician is called.
- For surveying work, a surveyor is brought in.

Python works in a very similar way.

Python itself is like the manager — it coordinates the work, but specialised tasks are handled by expert modules called *packages* / *libraries*.

When we write:

```python
import ifcopenshell
```
we are telling Python:

> "Bring in the BIM expert."

IfcOpenShell is a specialised library that understands the IFC standard used in Building Information Modelling (BIM). It knows how to create, read, modify, and organise building data such as walls, slabs, columns, storeys, and relationships between them.

So this line is not merely loading code — it is:

* requesting a BIM specialist,
* adding IFC capabilities to Python,
* and enabling Python to work intelligently with digital building models.

---

## 2. Starting a New Project
In computer, every building project will start with creating a computer model.

```python
# Create a new IFC file (IFC4 schema)
model = ifcopenshell.file(schema="IFC4")
```
Explanation:
> This line creates a brand-new, empty digital file. The term `schema="IFC4"` is like choosing the building code or the version of the manual you are following. IFC4 is the latest standards in the industry.

---

## 3. Creating the Building Components
In civil engineering, we organise things logically: Project → Site → Building → Floor. We call these "Entities."

```python
project = model.create_entity("IfcProject", GlobalId=ifcopenshell.guid.new(), Name="Hut Project")
```
This creates the "Project" folder. The `GlobalId` is like a unique Social Security Number for the project so the computer never confuses it with another one.

```python
site = model.create_entity("IfcSite", GlobalId=ifcopenshell.guid.new(), Name="Security Yard")
building = model.create_entity("IfcBuilding", GlobalId=ifcopenshell.guid.new(), Name="Cabin")
storey0 = model.create_entity("IfcBuildingStorey", GlobalId=ifcopenshell.guid.new(), Name="Ground Floor")
```
Explanation:
> Here, we define the physical "levels" of our model:
* **Site:** The actual piece of land or plot.
* **Building:** The physical structure.
* **Storey0:** The specific floor level (Ground Floor).

---

## 4. Building the Hierarchy (Relationships)
In the real world, a floor is inside a building, and a building is on a site. Computers don't know this automatically; we have to tell them using "Relationships."

```python
model.create_entity(
    "IfcRelAggregates",
    GlobalId=ifcopenshell.guid.new(),
    RelatingObject=project,
    RelatedObjects=[site]
)
```
Explanation:
> The word `IfcRelAggregates` is technical jargon for "This belongs to that." In this block, we are telling the computer: "The **Site** is part of the **Project**."

```python
model.create_entity(
    "IfcRelAggregates",
    GlobalId=ifcopenshell.guid.new(),
    RelatingObject=site,
    RelatedObjects=[building]
)
```
Explanation:
> Similarly, this tells the computer: "The **Building** is located on this **Site**."

```python
model.create_entity(
    "IfcRelAggregates",
    GlobalId=ifcopenshell.guid.new(),
    RelatingObject=building,
    RelatedObjects=[storey0]
)
```
> Explanation:
Finally, this links the **Ground Floor** (storey /  Level 0) of the **Building**. It creates a logical chain from the largest concept down to the specific floor.

---

## 5. Saving Your Work
Once the data is structured, we must save it to a file that other engineering software (like Revit or ArchiCAD) can open.

```python
# Save IFC file
model.write("hut.ifc")

print("IFC file created: hut.ifc")
```
Explanation:
>The `model.write` command acts like the "Save As" button. It creates a physical file named `hut.ifc` on your computer. The `print` command simply sends a message to your screen to let you know the process is finished successfully.

---

## Summary Table for Laypeople

| Concept | Building Equivalent | Technical Name |
| :--- | :--- | :--- |
| Toolkit | Construction Tools | `import ifcopenshell` |
| Plot of Land | The Construction Site | `IfcSite` |
| Structure | The actual house/office | `IfcBuilding` |
| Levels | Ground Floor, 1st Floor | `IfcBuildingStorey` |
| Connection | Glue or Nails (Holding it together) | `IfcRelAggregates` |

---

## Complete Code for Copy-Paste
You can copy the block below and run it in your Python environment to generate your first BIM model.

```python
import ifcopenshell

# Create a new IFC file (IFC4 schema)
model = ifcopenshell.file(schema="IFC4")

# Create basic entities
project = model.create_entity("IfcProject", GlobalId=ifcopenshell.guid.new(), Name="Hello World Project")

site = model.create_entity("IfcSite", GlobalId=ifcopenshell.guid.new(), Name="My Site")
building = model.create_entity("IfcBuilding", GlobalId=ifcopenshell.guid.new(), Name="My Building")
storey0 = model.create_entity("IfcBuildingStorey", GlobalId=ifcopenshell.guid.new(), Name="Ground Floor")

# Define relationships (hierarchy)
model.create_entity(
    "IfcRelAggregates",
    GlobalId=ifcopenshell.guid.new(),
    RelatingObject=project,
    RelatedObjects=[site]
)

model.create_entity(
    "IfcRelAggregates",
    GlobalId=ifcopenshell.guid.new(),
    RelatingObject=site,
    RelatedObjects=[building]
)

model.create_entity(
    "IfcRelAggregates",
    GlobalId=ifcopenshell.guid.new(),
    RelatingObject=building,
    RelatedObjects=[storey0]
)

# Save IFC file
model.write("hut.ifc")

print("IFC file created: hut.ifc")
```

## IFC versions

Historical overview of the IFC standard from buildingSMART International.

| IFC Version                   | Release Year | Key Features / Improvements                                                                                                                                                   |
| ----------------------------- | -----------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IFC 1.0                       |         1997 | First public IFC release. Basic object-oriented BIM data model for buildings. Experimental and limited adoption.                                                              |
| IFC 1.5                       |         1998 | Improved interoperability and geometry handling. Better support for architectural elements and relationships.                                                                 |
| IFC 2.0                       |         1999 | Major schema restructuring. Expanded support for building services and structural concepts.                                                                                   |
| IFC 2x                        |         2000 | Introduced wider interoperability concepts and improved model consistency. Became more practically usable.                                                                    |
| IFC 2x2                       |         2003 | Better geometric representations and property handling. Improved exchange reliability between software platforms.                                                             |
| IFC 2x3                       |         2006 | Most widely adopted legacy version. Strong support for architecture, structure, MEP, quantities, and property sets. Became industry baseline for many years.                  |
| IFC 2x3 TC1                   |         2007 | Technical corrigendum release. Bug fixes, clarification of schema ambiguities, and improved implementation consistency.                                                       |
| IFC 4                         |         2013 | Major modernization. Improved geometry kernel, better parametric definitions, enhanced material handling, NURBS support, constraints, and stronger extensibility.             |
| IFC 4 ADD1                    |         2014 | Addendum release with corrections and implementation refinements for IFC4 adoption.                                                                                           |
| IFC 4 ADD2                    |         2016 | Improved stability and software interoperability. Became the practical IFC4 baseline used by many vendors.                                                                    |
| IFC 4 ADD2 TC1                |         2017 | Technical corrections and clarifications to IFC4 ADD2.                                                                                                                        |
| IFC 4.1                       |         2018 | Extension toward infrastructure and civil engineering domains. Better alignment with roads, rail, and bridges. Transitional release.                                          |
| IFC 4.2                       |         2019 | Expanded infrastructure capabilities. Improvements for alignment, linear infrastructure, ports, waterways, and georeferencing.                                                |
| IFC 4.3 RC series             |    2020–2022 | Release candidate phase for major infrastructure expansion. Heavy testing and vendor implementation efforts.                                                                  |
| IFC 4.3                       |         2023 | Landmark infrastructure release. Native support for roads, bridges, railways, tunnels, ports, and civil assets. Improved alignment-based modeling and geospatial integration. |
| IFC 4.3.1 / ongoing revisions |    2024–2026 | Continued stabilization, bug fixes, domain refinements, and better interoperability for infrastructure BIM workflows.                                                         |

---

# Most Important Versions in Practice

| Version | Importance                                    |
| ------- | --------------------------------------------- |
| IFC 2x3 | Legacy industry workhorse; still heavily used |
| IFC 4   | Modern BIM baseline                           |
| IFC 4.3 | Major civil/infrastructure milestone          |

---

# Civil Engineering Perspective

## IFC 2x3 Era

Focused mainly on:

* Buildings
* Architectural BIM
* Structural framing
* MEP

Civil infrastructure support was limited.

---

## IFC 4 Era

Introduced:

* Better geometry
* Better materials
* Better parametric representation
* Improved extensibility

Good foundation for advanced BIM workflows.

---

## IFC 4.3 Era

Critical for civil engineers because it adds:

* Roads
* Bridges
* Railways
* Tunnels
* Alignment-based infrastructure
* Linear referencing
* Geospatial integration

This is the version where IFC became truly infrastructure-capable.

---

# Simplified Evolution

```text
IFC 1.x   → Experimental BIM
IFC 2x3   → Building BIM maturity
IFC 4     → Modernized BIM core
IFC 4.3   → Infrastructure & civil BIM
```

These tools increasingly target IFC4 and IFC4.3 workflows.
