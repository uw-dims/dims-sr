.. _scope:

Scope
=====

.. _identification:

Identification
--------------

This System Requirements document (version |release|) describes
the high-level requirements for designing and implementing
the Distributed Incident Management System (DIMS).

.. _systemoverview:

System overview
---------------

DIMS is funded by the Department of Homeland Security under contract HSHQDC-
13-C-B0013. For more information, see the document, "System Requirements and
Concept of Operations for From Local to Gobal Awareness: A Distributed Incident
Management System (DIMS)" referenced in Section :ref:`referenceddocs`.

The primary mission objectives for the DIMS system are operational in nature,
focused on facilitating the exchange of operational intelligence and applying
this intelligence to more efficiently respond and recover from cyber
compromise. The secondary mission objectives are to create a framework in which
tools to support the primary mission objectives can more quickly and easily be
integrated and brought to bear against advancing techniques on the attacker
side of the equation.

The DIMS project is intended to take this semi-automated sharing of structured
threat information, building on the success of the Public Regional Information
Security Event Monitoring (PRISEM) project and leveraging an existing community
of operational security professionals known as Ops- Trust, and scale it to the
next level. The intent of this software project is to allow for near real-time
sharing of critical alerts and structured threat information that will allow
each contributing party to receive information, alerts and data, analyze the
data, and respond appropriately and in a timely manner through one
user-friendly web application, DIMS.

Working with the use cases defined by MITRE and PRISEM users, building the
features necessary to simplify structured information sharing, and
operationalizing these within these existing communities, will allow DIMS to
fill existing gaps in capabilities and support existing missions that are
slowed down today by many complicated, manual processes.

The changes to existing systems consists of seamless integration of the three
current systems into a single web application that enables each system to
contribute to the data warehouse of information concerning threats, alerts,
attacks and suspect or compromised user terminals within the infrastructure.
Additionally, the integrated systems will be able to share and retrieve data,
visually observe alerts through color coded visual indicators, while retaining
the existing functionality of the current system.

.. _documentoverview:

Document overview
-----------------

The structure of this document has been adapted principally from MIL-STD-498
(see Section :ref:`referenceddocs`). Following this section are:

+ Section :ref:`referenceddocs` lists related documents.

+ Section :ref:`requirements` specifies the requirements for each major
  DIMS CSCI component. It includes descriptions of the high-level CSCI
  components (per contract) along with such specific requirements including
  (but not limited to): capabilities; interfaces; data inputs/outputs; adaptations;
  environment; resources; software quality factors; and design and implementation
  constraints.

+ Section :ref:`qualificationprovisions` provides qualification provisions that
  ensure that each requirement is met in the final system.

+ Section :ref:`notes` provides an alphabetical listing of acronyms and
  abbreviations used in this document.

+ Section :ref:`license` includes the copyright and software license under
  which DIMS is being released.
