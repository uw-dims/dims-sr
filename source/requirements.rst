.. _requirements:

Requirements
============

.. todo::

    This section shall be divided into the following paragraphs to specify the
    CSCI requirements, that is, those characteristics of the CSCI that are
    conditions for its acceptance. CSCI requirements are software requirements
    generated to satisfy the system requirements allocated to this CSCI. Each
    requirement shall be assigned a project-unique identifier to support
    testing and traceability and shall be stated in such a way that an
    objective test can be defined for it. Each requirement shall be annotated
    with associated qualification method(s) (see section 4) and traceability to
    system (or subsystem, if applicable) requirements (see section 5.a) if not
    provided in those sections.

    .. attention::

       The degree of detail to be provided shall be guided by the following
       rule: **Include those characteristics of the CSCI that are conditions
       for CSCI acceptance; defer to design descriptions those characteristics
       that the acquirer is willing to leave up to the developer.  If there are
       no requirements in a given paragraph, the paragraph shall so state. If a
       given requirement fits into more than one paragraph, it may be stated
       once and referenced from the other paragraphs.**

   ..

..

.. _statesandmodes:

Required states and modes
-------------------------

.. todo::

    .. attention::

        If the CSCI is required to operate in more than one state or mode
        having requirements distinct from other states or modes, this paragraph
        shall identify and define each state and mode. Examples of states and
        modes include: idle, ready, active, post-use analysis, training,
        degraded, emergency, backup, wartime, peacetime. The distinction
        between states and modes is arbitrary. A CSCI may be described in terms
        of states only, modes only, states within modes, modes within states,
        or any other scheme that is useful. If no states or modes are required,
        this paragraph shall so state, without the need to create artificial
        distinctions. If states and/or modes are required, each requirement or
        group of requirements in this specification shall be correlated to the
        states and modes. The correlation may be indicated by a table or other
        method in this paragraph, in an appendix referenced from this
        paragraph, or by annotation of the requirements in the paragraphs where
        they appear.

    ..

..

.. _modetoggles:

Mode toggles
~~~~~~~~~~~~

Modes described in this section can be be toggled **on** or **off** from the
user dashboard interface. When toggeled, DIMS subsystems will either be made
aware of the change (e.g., through the AMQP message bus, by monitoring a
distributed file system path, or by checking for the presense of a flag
indicating the state of the toggle on starteup). If notification of the change
is not made (in a "push" fashion), DIMS subsystems will need to actively poll
for the change.

Since DIMS is designed to accumulate event logs, this is just a variation of
the existing event log collection mechanisms built into DIMS. Test and/or debug
logs will be tagged so as to separate them from the security event logs. A
mechanism to purge logs that are no longer necessary should be supported.
(This may be an optional setting when toggles are turned **off**.)

.. _testmode:

Test Mode
~~~~~~~~~

The DIMS system shall be designed to support a test mode that generates
information useful for tests at the system level as described in the
DIMS Test Plan, section :ref:`dimstp:testlevels`.

.. todo::

   Expand on what this mode should do.

..

.. _debugmode:

Debug Mode
~~~~~~~~~~

The DIMS system shall be designed to support a debugging mode that allows
generation of increasingly verbose logs that will assist in debugging.

.. todo::

   Show example of what this kind of output looks like from the PRISEM
   utilities.

..


.. _demomode:

Demonstration Mode
~~~~~~~~~~~~~~~~~~

In order to demonstrate a live instance of the DIMS system, without exposing
any sensitive information it may contain, the DIMS system should support a
demonstration mode that loads specially prepared **demonstration**
data. This data may be fabricated, manually anonymized and/or collected
from honeypot systems that are outside of any sensitive network blocks.
This mode will also be useful for teaching students how to become analysts.

During normal use of the DIMS system with live data and anonymization turned
on, the user may chose to save interim search results or other analysis
products from the data processing stream to a separate storage location
for use in demonstration mode. This should include the ability to export
all of the data in a single archive file for simplicity in building a
library of demonstration data. This data can then be made available
along with the DIMS software and deployment utilities so someone can
easily bring up a demonstration instance with little or no manual
intervention.

.. note::

   For an example of what this would look like, see how MozDef or GRR work by
   building and running their respective Docker images as described
   in their documentation. DIMS will model these projects in production
   of a simple demo-mode deployment.

..

When in demonstration mode, the system should take the set of search parameters
that are given to the user interface and generate a hash to save with the
results. When in debug mode later, the set of search hashes can be used to
either pre-populate the user interface, or just be used to compare with later
searches done in Demonstration mode. When it is recognized that a pre-recorded
search is being initiated, rather than send the paramters off to search
processors, the system can retrieve the saved search results related to the
search hash and present them to the user. This allows the system to appear to
function as normal, but without having to fill the databases with fake data.
(This also allows a production system to be used in demonstration mode without
polluting the production databases.)

When done using demonstration mode, an additional option to save or delete any
saved files should be supported to simulate multi-session and multi-user
use of DIMS. When selecting deletion, there are two sub-states:

#. Selecting **clean all** will clean out all of the demonstration data and
   intermediary results, allowing a production system to be completely
   cleaned of any demostration related data;

#. Selecting **clean temporary files** only deletes the intermediary saved
   results, not the original demonstration data. This resets the demonstration
   back to the start for predictable repetition.

If it is easier, both of these sub-modes can involve complete deletion of
a single demonstration partitioned datastore (only the first mode would
immediately re-load the demonstration datastore from the original demonstration
dataset.

.. _capabilityrequirements:

CSCI capability requirements
----------------------------

The DIMS system is divided into the following high-level CSCI sets,
per the acquisition contract referenced in Sections :ref:`systemoverview`
and :ref:`referenceddocs`.

===================================== ========= =============
CSCI                                  Label     Contract Item
===================================== ========= =============
Backend Data Stores                   BDS       C.3.1.1
Dashboard Web Application             DWA       C.3.1.1
Data Integration and User Tools       DIUT      C.3.1.2
Vertical/Lateral Information Sharing  VLIS      C.3.1.3
===================================== ========= =============

This subsection is divided into subparagraphs to itemize the
requirements associated with each capability of these CSCI sets.
Each capability is labelled with its specific CSCI

.. _bdscsci:

Backend Data Stores CSCI - (BDS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following sections describe the requirements for the Backend
Data Stores (BDS) CSCI.

.. todo::

    .. attention::

        This paragraph shall identify a required CSCI capability and shall
        itemize the requirements associated with the capability. If the
        capability can be more clearly specified by dividing it into
        constituent capabilities, the constituent capabilities shall be
        specified in subparagraphs. The requirements shall specify required
        behavior of the CSCI and shall include applicable parameters, such as
        response times, throughput times, other timing constraints, sequencing,
        accuracy, capacities (how much/how many), priorities, continuous
        operation requirements, and allowable deviations based on operating
        conditions. The requirements shall include, as applicable, required
        behavior under unexpected, unallowed, or "out of bounds" conditions,
        requirements for error handling, and any provisions to be incorporated
        into the CSCI to provide continuity of operations in the event of
        emergencies. Paragraph 3.3.x of this DID provides a list of topics to
        be considered when specifying requirements regarding inputs the CSCI
        must accept and outputs it must produce.

    ..

..

.. _attributestorage:

Attribute Storage
^^^^^^^^^^^^^^^^^

The DIMS system must have the ability to store additional attributes for each
user (such as which CIDR blocks they are responsible for protecting, which top
level Domain Name System domains, and/or which high-level activities (e.g.,
campaigns) they wish to monitor. This capability allows the system to notify
the user when there are messages or email threads of interest, and to
facilitate providing regular tailored reports or alerts about activity of
interest to them. These attributes also support the basis for role-based access
controls. This real-time situational awareness capability is one of the most
important features that will improve response and reaction time, as it removes
the necessity to read and process every single message that flows through the
system at a given time, or to manually trigger reports or searches to get
situational awareness.

.. _bdsuserstory1:

BDS User Story 1
^^^^^^^^^^^^^^^^

"As {an investigator, analyst} I want to be able to preserve the results of
searches, and in some cases the data that was identified while searching, in
order to have copies that are subject to expiration and purging from the
system. Some investigations may take many months, which could bump up against
the data retention period (approximately 12 months, at present)."

.. bdsuserstory2:

BDS User Story 2
^^^^^^^^^^^^^^^^

"As {a security operator, investigator, analyst, CISO} I want to be able to
define multiple sets of attributes that the system can then use to inform me
about when new data is seen that matches those attributes. Attributes can
include anything that might be seen in indicators of compromise, observables,
or alerts. (The most basic being IP addresses and/or CIDR blocks, domain names,
MD5 or other cryptographic hash values, file names, Registry key settings,
etc.)"


.. _dwacsci:

Dashboard Web Application CSCI - (DWA)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Dashboard Web Application, also referred to as the DIMS Dashboard, 
provides a multi-faceted user interface and data visualization and
analytic tools to integrate data from many sources and facilitate 
trusted information sharing. The following subsections contain the
user stories which describe the Dashboard Web Application requirements.

.. todo::

    .. attention::

        This paragraph shall identify a required CSCI capability and shall
        itemize the requirements associated with the capability. If the
        capability can be more clearly specified by dividing it into
        constituent capabilities, the constituent capabilities shall be
        specified in subparagraphs. The requirements shall specify required
        behavior of the CSCI and shall include applicable parameters, such as
        response times, throughput times, other timing constraints, sequencing,
        accuracy, capacities (how much/how many), priorities, continuous
        operation requirements, and allowable deviations based on operating
        conditions. The requirements shall include, as applicable, required
        behavior under unexpected, unallowed, or "out of bounds" conditions,
        requirements for error handling, and any provisions to be incorporated
        into the CSCI to provide continuity of operations in the event of
        emergencies. Paragraph 3.3.x of this DID provides a list of topics to
        be considered when specifying requirements regarding inputs the CSCI
        must accept and outputs it must produce.

    ..

..

.. _dwauserstory1:

DWA User Story 1
^^^^^^^^^^^^^^^^

"As {an investigator, analyst} I want to be able to keep track of cases and
campaigns (i.e., groups of related incidents). I want the system to inform me,
if I so chose, of any time new data that is determined to be associated with
the sets I am tracking comes into the system. For example, if I log in and open
a case, I can easily tell which data has been entered into the case since the
last time viewed the case. This allows me to stay on top of new evidence or
activity that I am investigating."

.. _dwauserstory2:

DWA User Story 2
^^^^^^^^^^^^^^^^

"As {a security operator, investigator} I want to be told when an email thread
or received set of indicators includes systems that I am responsible for
securing, ideally pointing out to me those hosts that are involved without
requiring that I read the entire thread, extract attachments, write scripts to
parse and search data, etc. I want to be given a list of those records that are
important, in a format that I can submit directly to query interfaces without
having to write scripts to parse and process."

.. _dwauserstory3:

DWA User Story 3
^^^^^^^^^^^^^^^^

"As an {analyst, investigator, security operator}, I would like to be able to
get context about 'external' hosts that includes what kind of malicious
activity has been observed, by whom, starting and ending when, have they been
involved in precious incidents I have dealt with, etc. This view could combine
a timeline aspect (first seen to last seen time ranges along the X axis), for
one or more sources of threat intelligence (discrete items along a non-linear Y
axis) with some method of mapping to these external hosts (grouping into AS,
etc.). The objective is to quickly associate context about threats within
observed flows or logged events."

.. _dwauserstory4:

DWA User Story 4
^^^^^^^^^^^^^^^^

"As an {analyst, investigator, security operator}, I would like to be able to
step through large volumes of output records in a manner that reduces the set
of remaining items as quickly as possible. I would like to see related entries
visually identified as being part of a common set, and have the ability to
select one representative entry, tag it, categorize it as being benign or
malicious, then filtering all of the related records out so as to focus on
categorizing the remaining records. If the system can remember the tags and
automatically apply them when similar records are seen in the future, it will
be easier to identify new unknown records that require analytic scrutiny."

.. _dwauserstory5:

DWA User Story 5
^^^^^^^^^^^^^^^^

"As an {analyst, security operator}, I would like to have links to detailed
analyses and reports that are available in public sources when a query I have
made results in identifying known malware or malicious actors. This way I can
more quickly come up to speed on what is (or is not) known about the threat
behind the indicators or observables I am dealing with."

.. _dwauserstory6:

DWA User Story 6
^^^^^^^^^^^^^^^^

"As a {system administrator, security operator, network operator}, I would like
to have links to Course of Action steps related to the threats that I identify
using the DIMS system. This allows me to not only inform owners or compromised
assets that have been identified by the system, but to also give them
information about what they need to do, in what order they should take steps,
and when/how to preserve evidence in the event that there is criminal
investigation ongoing."

.. _dwauserstory7:

DWA User Story 7
^^^^^^^^^^^^^^^^

"As an {analyst, security operator, investigator, network operator, system
administrator}, I would like to be able to have access to DIMS functions
via an intuitive web user interface."

.. _dwauserstory8:

DWA User Story 8
^^^^^^^^^^^^^^^^

"As a system administratory, I want the DIMS Dashboard to report information
upon system startup and at periodic intervals that indicate operational status."


.. _diutcsci:

Data Integration and User Tools CSCI - (DIUT)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following sections describe the requirements for the Data
Integration and User Tools (DIUT) CSCI.

.. todo::

    .. attention::

        This paragraph shall identify a required CSCI capability and shall
        itemize the requirements associated with the capability. If the
        capability can be more clearly specified by dividing it into
        constituent capabilities, the constituent capabilities shall be
        specified in subparagraphs. The requirements shall specify required
        behavior of the CSCI and shall include applicable parameters, such as
        response times, throughput times, other timing constraints, sequencing,
        accuracy, capacities (how much/how many), priorities, continuous
        operation requirements, and allowable deviations based on operating
        conditions. The requirements shall include, as applicable, required
        behavior under unexpected, unallowed, or "out of bounds" conditions,
        requirements for error handling, and any provisions to be incorporated
        into the CSCI to provide continuity of operations in the event of
        emergencies. Paragraph 3.3.x of this DID provides a list of topics to
        be considered when specifying requirements regarding inputs the CSCI
        must accept and outputs it must produce.

    ..

..

.. _incidenttracking:

Incident/Campaign Tracking
^^^^^^^^^^^^^^^^^^^^^^^^^^

The DIMS system must be able to keep track of multiple incidents, campaigns,
sector-specific threat activity, or other ad-hoc groupings of security
information as desired by DIMS users. For example, an analyst may wish to track
ZeroAccess trojan activity, CryptoLocker extortion attempts, Zeus or Citadel
ACH fraud attempts, etc., possibly over time periods measured in years. Each
user may wish to label these associated sets with their own labels, or may want
to use a system-wide naming scheme that conforms to an ontology that is more
rigorously defined. These sets should be easily shared with other users.

.. _knowledgeacquisition:

Knowledge Acquisition
^^^^^^^^^^^^^^^^^^^^^

The DIMS system should support knowledge acquisition by allowing the user to be
told, on login and when they focus on a particular incident or campaign, what
new information has been obtained from other users of the system (or the system
itself through automated detection and reporting) since the last time the user
was reviewing the incident or campaign. Collaboration works best when team
members learn from each other, and the asynchronous nature of a multi-user
system is such that determining the delta in knowledge since an earlier point
in time is difficult to achieve.

.. todo::

    .. attention::

        Update this reference, or remove: "(This is related to the issue of
        tracking incoming information in email threads listed earlier.)

    ..

..

.. _aggregatesummary:

Summarize Aggregate Data
^^^^^^^^^^^^^^^^^^^^^^^^

The DIMS system should summarize any/all aggregate data that any user is
presented with sufficient context to quickly understand the data. This includes
(but is not limited to): Start and end date and time; Total number of systems
within the "friend" population, and how they break down across participants;
Total number of systems outside of the "friend" population, and how they break
down by country/AS/IP address(es); Total number of systems from the
"not-friend" population that are known to be malicious (a.k.a., "foe"), broken
down by country/AS/IP address(es). When the number of IP addresses exceeds a
certain threshold, they are summarized in aggregate, with a mechanism to dig
down if the user so chooses. Similarly, context about what quantity and quality
of malicious activity that is known about the "foe" population should also be
available for easy access (presented if short, or drill-down provided it too
voluminous). This amount and level of detail provides an overall "situational
awareness" or scoping of for large volumes of security event data. (The
mechanism for such multi-level tabular reports is known as "break" or "step"
reports).

.. todo::

   Put in the references to break and step reports.

..

.. _diutuserstory1:

DIUT User Story 1
^^^^^^^^^^^^^^^^^

"As an investigator, I would like to be able to timestamp files I create (i.e.,
calculate multiple different cryptographic hashes of the contents of files to
validate their integrity, associate a timestamp from a trusted time source,
then cryptographically sign the result with a private key). This allows
validation of the existence of a file at a point in time, who produced the
file, and maintenance of a form of "chain of custody" of the contents of the
file. To ensure privacy as well as integrity and provenance, the file would
first be encrypted (or both cleartext and encrypted files included in the
timestamping operation)."

.. _diutuserstory2:

DIUT User Story 2
^^^^^^^^^^^^^^^^^

"As a system administrator, I would like to have a picture of the operational
state of all of the system components that make up DIMS (and related underlying
SIEM, etc.) This will allow me to quickly diagnose outages in dependent
sub-systems that cause the system as a whole to not function as expected. The
less time that it takes me to diagnose the trouble and remediate, the better."

.. _diutuserstory3:

DIUT User Story 3
^^^^^^^^^^^^^^^^^

"As a system administrator, I would like to be able to update or reconfigure
DIMS subsystem components from a central location (rather than having to log in
to each system and copy/edit files by hand). I would like to be assured that
those changes are applied uniformly across all subsystem components, and that I
have a mechanism to back out to a previous running state if need be to maintain
uptime."

.. _diutuserstory4:

DIUT User Story 4
^^^^^^^^^^^^^^^^^

"As a {system administrator, security operator}, I would like to know that the
DIMS system components are being monitored for attempted access by any of the
same malicious actors who are seen to be threatening my constituent users. It
is only natural to assume that an attack on any participant site could lead to
discovery of the security monitoring system and for that system to be attacked
as well, so the system should be monitoring itself using the same
cross-organizational correlation features as are used internally."

.. _diutuserstory5:

DIUT User Story 5
^^^^^^^^^^^^^^^^^

"As a system administrator, I would like to be able to deal with a breach of
the security system in a tactical way. If a user is found to have had a
compromise of their account, all access to that user should be disabled
uniformly across all system components via the single-signon authentication
subsystem. All cryptograph keys should also be revoked. Once the user has been
informed and the computer systems they use cleaned, all cryptographic keys,
certificates, and password should be updated and re-issued."

.. _diutuserstory6:

DIUT User Story 6
^^^^^^^^^^^^^^^^^

"As a {system administrator, security operator}, I would like to be able to
link indicators and observables that come in at the network level (e.g., IP
addresses, domain names, URLs) to observables at the host level (e.g., Registry
Keys and values, file names, cryptographic hashes of files) and search for
those observables to confirm or refute assertions that computers under my
authority have been compromised. If I get confirmation, I would then like to
preserve evidence and maintain chain of custody for that evidence as easily and
quickly as possible."

.. _diutuserstory7:

DIUT User Story 7
^^^^^^^^^^^^^^^^^

"As an {analyst, security operator} I would like to be able to start an
analysis and annotate data files as I go through the analysis process, trying
to derive meaning from what I am seeing in the data, and being able to (at any
time seems appropriate) create a reference to the current data set(s) and my
view of them so I can pass this reference identifier to another analyst, a
CISO, or an investigator, to allow them to take a look at what I am seeing and
provide their input. For example, if someone reports a DoS attack directed at
SLTT government, and my analysis confirms that such an act can be seen in the
PRISEM population, I would like to provide my observations to someone to help
investigate targeting, etc., in order to develop a better picture of what is
happening. If the result is a determination that a SITREP should be developed
and information passed along to federal law enforcement, the updated annotated
body of data can then be assembled into a SITREP (using a 'break' or 'step'
reporting format, including both cleartext and anonymized versions for sharing
with outside groups) and passed along with little added effort."

.. _diutuserstory8:

DIUT User Story 8
^^^^^^^^^^^^^^^^^

"As a user of the system, I would like to see the status of any asynchronous
queries or report generation requests I have made. It is reasonable for a
search through the entire history of billions of events to take some time to
complete, but I would like to be able to tell approximately how long I will
have to wait. Ideally, the system would keep track of previous requests, the
time span and complexity of filtering applied, and to provide a time estimate
when a new query is being formulated so as to guide me in deciding what I
really need to ask for to get an answer in the time frame I am faced with at
the moment."


.. _vliscsci:

Vertical/Lateral Information Sharing CSCI - (VLIS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The following sections describe the requirements for the Vertical
and Lateral Information Sharing (VLIS) CSCI.

.. todo::

    .. attention::

        This paragraph shall identify a required CSCI capability and shall
        itemize the requirements associated with the capability. If the
        capability can be more clearly specified by dividing it into
        constituent capabilities, the constituent capabilities shall be
        specified in subparagraphs. The requirements shall specify required
        behavior of the CSCI and shall include applicable parameters, such as
        response times, throughput times, other timing constraints, sequencing,
        accuracy, capacities (how much/how many), priorities, continuous
        operation requirements, and allowable deviations based on operating
        conditions. The requirements shall include, as applicable, required
        behavior under unexpected, unallowed, or "out of bounds" conditions,
        requirements for error handling, and any provisions to be incorporated
        into the CSCI to provide continuity of operations in the event of
        emergencies. Paragraph 3.3.x of this DID provides a list of topics to
        be considered when specifying requirements regarding inputs the CSCI
        must accept and outputs it must produce.

    ..

..

.. _structuredinput:

Structured data input
^^^^^^^^^^^^^^^^^^^^^

The DIMS system must have the ability to process structured data that is
entered into the system in one of several ways: (1) attached to email messages
being sent to the Ops-Trust portal (optionally as encrypted attachments); (2)
via CIF feed, TAXII, AMQP message bus, or other asynchronous automated
mechanism; (3) as uploaded from a user’s workstation via the DIMS dashboard
client; (4) via the Tupelo client or other command line mechanism.

.. _assetidentification:

Asset Identification
^^^^^^^^^^^^^^^^^^^^

The DIMS system must be able to detect when IP addresses or domain names
associated with a given set of CIDR blocks or top-level domains are involved,
and to trigger one or more workflow processes. This could be to send an alert
to a user when some entity they are watching is found in a communication,
generate a scheduled report, or trigger some other asynchronous event. It may
be to initiate a search of available data so the results can be ready for a
user to view when they receive the alert, rather than requiring that they
initiate a search at that time and have to wait for the results.

.. _vlisuserstory1:

VLIS User Story 1
^^^^^^^^^^^^^^^^^

"As a user of the DIMS system, I would like the ability to (at any point in
time during analysis of an incident or while viewing the situation associated
with threats across the user population) produce an anonymized version of the
output I am looking at so as to be able to share it with outside entities. The
system should anonymize and filter the data according to the policies set by
the entities that provided the underlying data, and I should be able to
determine the policy for sharing of information (by clearly seeing its tagged
TLP sensitivity level). Reports should similarly be tagged appropriately with
TLP for the sensitivity level of the aggregate document."


.. _externalrequirements:

CSCI external interface requirements
------------------------------------

.. todo::

    .. attention::

        This paragraph shall be divided into subparagraphs to specify the
        requirements, if any, for the CSCI's external interfaces. This
        paragraph may reference one or more Interface Requirements
        Specifications (IRSs) or other documents containing these requirements.

    ..
..

.. _interfaceid:

Interface identification and diagrams
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. todo::

    .. attention::

        This paragraph shall identify the required external interfaces of the
        CSCI (that is, relationships with other entities that involve sharing,
        providing or exchanging data). The identification of each interface
        shall include a project-unique identifier and shall designate the
        interfacing entities (systems, configuration items, users, etc.) by
        name, number, version, and documentation references, as applicable.
        The identification shall state which entities have fixed interface
        characteristics (and therefore impose interface requirements on
        interfacing entities) and which are being developed or modified (thus
        having interface requirements imposed on them).  One or more interface
        diagrams shall be provided to depict the interfaces.

    ..

..

.. _interfacepuid:

(Project unique identifier of interface)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. todo::

    .. attention::

        This paragraph (beginning with 3.3.2) shall identify a CSCI external
        interface by project unique identifier, shall briefly identify the
        interfacing entities, and shall be divided into subparagraphs as needed
        to state the requirements imposed on the CSCI to achieve the interface.
        Interface characteristics of the other entities involved in the
        interface shall be stated as assumptions or as "When [the entity not
        covered] does this, the CSCI shall...," not as requirements on the
        other entities. This paragraph may reference other documents (such as
        data dictionaries, standards for communication protocols, and standards
        for user interfaces) in place of stating the information here. The
        requirements shall include the following, as applicable, presented in
        any order suited to the requirements, and shall note any differences in
        these characteristics from the point of view of the interfacing
        entities (such as different expectations about the size, frequency, or
        other characteristics of data elements):

        #. Priority that the CSCI must assign the interface
        #. Requirements on the type of interface (such as real-time data
           transfer, storage-and-retrieval of data, etc.) to be implemented
        #. Required characteristics of individual data elements that the CSCI
           must provide, store, send, access, receive, etc., such as:
    
            #. Names/identifiers
    
                #. Project-unique identifier
                #. Non-technical (natural language) name
                #. DoD standard data element name
                #. Technical name (e.g., record or data structure name in code or
                   database)
                #. Abbreviations or synonymous names
    
            #. Data type (alphanumeric, integer, etc.)
            #. Size and format (such as length and punctuation of a character
               string)
            #. Units of measurement (such as meters, dollars, nanoseconds)
            #. Range or enumeration of possible values (such as 0-99)
            #. Accuracy (how correct) and precision (number of significant digits)
            #. Priority, timing, frequency, volume, sequencing, and other
               constraints, such as whether the data element may be updated and
               whether business rules apply
            #. Security and privacy constraints
            #. Sources (setting/sending entities) and recipients (using/receiving
               entities)
    
        #. Required characteristics of data element assemblies (records,
           messages, files, arrays, displays, reports, etc.) that the CSCI must
           provide, store, send, access, receive, etc., such as:
    
            #. Names/identifiers
    
                #. Project-unique identifier
                #. Non-technical (natural language) name
                #. Technical name (e.g., record or data structure name in code or
                   database)
                #. Abbreviations or synonymous names
    
            #. Data elements in the assembly and their structure (number, order,
               grouping)
            #. Medium (such as disk) and structure of data elements/assemblies on
               the medium
            #. Visual and auditory characteristics of displays and other outputs
               (such as colors, layouts, fonts, icons and other display elements,
               beeps, lights)
            #. Relationships among assemblies, such as sorting/access
               characteristics
            #. Priority, timing, frequency, volume, sequencing, and other
               constraints, such as whether the assembly may be updated and whether
               business rules apply
            #. Security and privacy constraints
    
        #. Required characteristics of communication methods that the CSCI
           must use for the interface, such as:
    
            #. Project-unique identifier(s)
            #. Communication links/bands/frequencies/media and their
               characteristics
            #. Message formatting
            #. Flow control (such as sequence numbering and buffer allocation)
            #. Data transfer rate, whether periodic/aperiodic, and interval
               between transfers
            #. Routing, addressing, and naming conventions
            #. Transmission services, including priority and grade
            #. Safety/security/privacy considerations, such as encryption, user
               authentication, compartmentalization, and auditing
    
        #. Required characteristics of protocols the CSCI must use for the
           interface, such as:
    
            #. Project-unique identifier(s)
            #. Priority/layer of the protocol
            #. Packeting, including fragmentation and reassembly, routing, and
               addressing
            #. Legality checks, error control, and recovery procedures
            #. Synchronization, including connection establishment, maintenance,
               termination
            #. Status, identification, and any other reporting features
    
        #. Other required characteristics, such as physical compatibility of
           the interfacing entities (dimensions, tolerances, loads, plug
           compatibility, etc.), voltages, etc.
    
    ..

..

.. _internalinterfacereqs:

CSCI internal interface requirements
------------------------------------

.. todo::

    This paragraph shall specify the requirements, if any, imposed on
    interfaces internal to the CSCI. If all internal interfaces are left to the
    design, this fact shall be so stated. If such requirements are to be
    imposed, paragraph 3.3 of this DID provides a list of topics to be
    considered.

..

.. _internaldatareqs:

CSCI internal data requirements
-------------------------------

.. todo::

    This paragraph shall specify the requirements, if any, imposed on data
    internal to the CSCI. Included shall be requirements, if any, on databases
    and data files to be included in the CSCI. If all decisions about internal
    data are left to the design, this fact shall be so stated. If such
    requirements are to be imposed, paragraphs 3.3.x.c and 3.3.x.d of this DID
    provide a list of topics to be considered.

..

.. _adaptationreqs:

Adaptation requirements
-----------------------

.. todo::

    This paragraph shall specify the requirements, if any, concerning
    installation-dependent data to be provided by the CSCI (such as site-
    dependent latitude and longitude or site-dependent state tax codes) and
    operational parameters that the CSCI is required to use that may vary
    according to operational needs (such as parameters indicating
    operation-dependent targeting constants or data recording).

..

.. _safetyreqs:

Safety requirements
-------------------

.. todo::

    This paragraph shall specify the CSCI requirements, if any, concerned with
    preventing or minimizing unintended hazards to personnel, property, and the
    physical environment. Examples include safeguards the CSCI must provide to
    prevent inadvertent actions (such as accidentally issuing an "auto pilot
    off" command) and non-actions (such as failure to issue an intended "auto
    pilot off" command). This paragraph shall include the CSCI requirements, if
    any, regarding nuclear components of the system, including, as applicable,
    prevention of inadvertent detonation and compliance with nuclear safety
    rules.

..

.. _securityreqs:

Security and privacy requirements
---------------------------------

.. todo::

    This paragraph shall specify the CSCI requirements, if any, concerned with
    maintaining security and privacy. These requirements shall include, as
    applicable, the security/privacy environment in which the CSCI must
    operate, the type and degree of security or privacy to be provided, the
    security/privacy risks the CSCI must withstand, required safeguards to
    reduce those risks, the security/privacy policy that must be met, the
    security/privacy accountability the CSCI must provide, and the criteria
    that must be met for security/privacy certification/accreditation.

..

.. _networkaccesscontrols:

Network Access Controls
~~~~~~~~~~~~~~~~~~~~~~~

Remote users need to access DIMS components in order to use the
system. Direct internet access is necessary for a limited
subset of DIMS components, while the remainder are to be
restricted to indirect access through the Dashboard Web
Application and ops-trust portal front end, or by restricted
access through a Virtual Private Network (VPN) connection.

#. The features described in :ref:`dwacsci` are to be accessible from the
   internet from a limited set of network ports.
   
#. The features described in :ref:`bdscsci` are primarily only accessible
   to other CSCI components on a restricted network and have little or no
   direct user interface, while some features described in :ref:`diutcsci`
   :ref:`vliscsci` may have user Command Line Interfaces (CLIs) or
   Application Programming Interfaces (APIs) accessible only
   when the user is connected by VPN, or through SSH tunneling.

Ideally, all internet access to user interfaces (either graphical or command
line) will be through a single IP address via direct connection, through a
proxy connection, or to firewalled hosts via Network Address Translation
(NAT) and/or Port Forwarding (a.k.a., Destination NAT or DNAT). This is to
reduce the number of internet routable IP addresses and DNS names for a
DIMS deployment to just one, as well as to simplify access control
and access monitoring.

.. _accountaccesscontrols:

Account Access Controls
~~~~~~~~~~~~~~~~~~~~~~~

All DIMS component services should have access controls allowing only
authorized users access. The primary mechanism for doing this is the
use of a *Single Sign-On* (`SSO`_) system and authentication
service.


.. _SSO: http://en.wikipedia.org/wiki/Single_sign-on

.. _secondfactorauth:

Second-factor authentication
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The DIMS system should support the use of *two factor authentication*.
The ops-trust portal code base supports:

#. Time-base One Time Password (`TOTP`_)
#. HMAC-based One Time Password (`HOTP`_)
#. Static single use codes (a list of codes you can use to authenticate if all else fails)

The principle supported application for two-factor authentication is
`Google Authenticator`_.


.. _TOTP: http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm
.. _HOTP: http://en.wikipedia.org/wiki/HMAC-based_One-time_Password_Algorithm
.. _Google Authenticator: http://en.wikipedia.org/wiki/Google_Authenticator


.. _accountsuspension:

Account suspension
~~~~~~~~~~~~~~~~~~

When an account is suspected of being compromised, all access for that user
should be suspended in a manner that is non-destructive (i.e., access is removed,
but no credentials or account contents are deleted.) This allows an account to
be toggled off while an investigation takes place, and back on again once the
account has been deemed secure. Use of a *single-signon* (SSO) mechanism can
facilitate this, but additional mechanisms to remove access must also be
taken into consideration. For example, SSL client certificates (e.g., those used
with OpenVPN).

.. _rekeying:

Rekeying
~~~~~~~~

Cryptographic keys are used for secure access to many DIMS components, including
SSH public/private key pairs, and SSL client certificates for OpenVPN access.
Certificates should be generated for the user automatically as a workflow process
step performed by the system when a new account is activated in the ops-trust portal.

There should also be a way for user certificates to be regenerated (e.g., when
someone's laptop is compromised by malware, or is lost/stolen), and a way to
selectively (or wholesale) regenerate certificates for any/all users (e.g.,
when a DIMS system component suffers a breach.)

These security mechanisms allow restoration of a secure system with the least
amount of time/energy as possible.



.. _environmentreqs:

CSCI environment requirements
-----------------------------

.. todo::

    This paragraph shall specify the requirements, if any, regarding the
    environment in which the CSCI must operate. Examples include the computer
    hardware and operating system on which the CSCI must run.  (Additional
    requirements concerning computer resources are given in the next
    paragraph.)

..

.. _compresourcereqs:

Computer resource requirements
------------------------------

.. todo::

    This paragraph shall be divided into the following subparagraphs.

..

.. _comphardwarereqs:

Computer hardware requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. todo::

    This paragraph shall specify the requirements, if any, regarding computer
    hardware that must be used by the CSCI. The requirements shall include, as
    applicable, number of each type of equipment, type, size, capacity, and
    other required characteristics of processors, memory, input/output devices,
    auxiliary storage, communications/network equipment, and other required
    equipment.

..

.. _compresrouceutilizationreqs:

Computer hardware resource utilization requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. todo::

    This paragraph shall specify the requirements, if any, on the CSCI's
    computer hardware resource utilization, such as maximum allowable use of
    processor capacity, memory capacity, input/output device capacity,
    auxiliary storage device capacity, and communications/network equipment
    capacity. The requirements (stated, for example, as percentages of the
    capacity of each computer hardware resource) shall include the conditions,
    if any, under which the resource utilization is to be measured.

..

.. _compsoftwarereqs:

Computer software requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. todo::

    This paragraph shall specify the requirements, if any, regarding computer
    software that must be used by, or incorporated into, the CSCI. Examples
    include operating systems, database management systems, communications/
    network software, utility software, input and equipment simulators, test
    software, and manufacturing software. The correct nomenclature, version,
    and documentation references of each such software item shall be provided.

..

.. _compcommsreqs:

Computer communications requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. todo::

    This paragraph shall specify the additional requirements, if any,
    concerning the computer communications that must be used by the CSCI.
    Examples include geographic locations to be linked; configuration and
    network topology; transmission techniques; data transfer rates; gateways;
    required system use times; type and volume of data to be
    transmitted/received; time boundaries for transmission/ reception/response;
    peak volumes of data; and diagnostic features.

..

.. _swqualityfactors:

Software quality factors
------------------------

.. todo::

    This paragraph shall specify the CSCI requirements, if any, concerned with
    software quality factors identified in the contract or derived from a
    higher level specification. Examples include quantitative requirements
    regarding CSCI functionality (the ability to perform all required
    functions), reliability (the ability to perform with correct, consistent
    results), maintainability (the ability to be easily corrected),
    availability (the ability to be accessed and operated when needed),
    flexibility (the ability to be easily adapted to changing requirements),
    portability (the ability to be easily modified for a new environment),
    reusability (the ability to be used in multiple applications), testability
    (the ability to be easily and thoroughly tested), usability (the ability to
    be easily learned and used), and other attributes.

..

.. _designcontraints:

Design and implementation constraints
-------------------------------------

.. _automatedprovisioning:

Automated Provisioning
~~~~~~~~~~~~~~~~~~~~~~

The DIMS server components must be provisioned, configured, and administered
from a single central location and pushed to servers in an automated fashion.
Manual configuration and patching of hosts takes too much expert system
administration knowledge, incurs too much system administration overhead, and
takes too long to recover from outages or system upgrades. The DIMS team will
be administering multiple instances of the DIMS system (for development, alpha
testing, beta testing, a "production" PRISEM instance for in-field test and
evaluation, and potentially 3-5 more instances at other regions (see the
Stakeholders section). It will be impossible to manually manage that many
deployments with current staffing levels.

.. _agiledevelopment:

Agile development
~~~~~~~~~~~~~~~~~

The system will be built using an Agile coding methodology, responding to user
feedback as quickly as possible to ensure maximum usability and scalability.
The desired release cycle (length of a "sprint") is 2-3 weeks.

.. _continuousintegration:

Continuous Integration/Continuous Delivery
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The systems running DIMS software must support continuous integration of code
releases, updating runtime executables, stopping and starting service daemons,
etc., in a controlled and repeatable manner. Runtime components must identify
the source code release from which they were built in order to track bugs and
features across multiple deployments with a regular release cycle.

.. _leverageopensource:

Leveraging open source components
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As much as possible, DIMS will be built through the (re)use of open source
components used by other projects that are being integrated into the DIMS
framework. For example, the `Collective Intelligence Framework`_ (CIF) v2 and
the Mozilla Defense Platform (`MozDef`_) both employ the `ELK stack`_ and
`RabbitMQ`_ in their demonstration implementations, and the original PRISEM
distributed data processing tools also used RabbitMQ. Rather than have two
separate instances of Elasticsearch running in virtual machines or containers
for MozDef and CIF, and two separate instances of RabbitMQ in virtual machines
or containers for PRISEM tools and MozDef, a common Elasticsearch cluster and
RabbitMQ cluster would be set up and shared with these (and any other open
source tools added later).

.. _Collective Intelligence Framework: http://code.google.com/p/collective-intelligence-framework/
.. _MozDef: http://mozdef.readthedocs.org/en/latest/
.. _ELK stack: http://www.elasticsearch.org/overview/
.. _RabbitMQ: http://www.rabbitmq.com/

.. _personnelreqs:

Personnel-related requirements
------------------------------

.. todo::

    This paragraph shall specify the CSCI requirements, if any, included to
    accommodate the number, skill levels, duty cycles, training needs, or other
    information about the personnel who will use or support the CSCI. Examples
    include requirements for number of simultaneous users and for built-in help
    or training features. Also included shall be the human factors engineering
    requirements, if any, imposed on the CSCI.  These requirements shall
    include, as applicable, considerations for the capabilities and limitations
    of humans; foreseeable human errors under both normal and extreme
    conditions; and specific areas where the effects of human error would be
    particularly serious. Examples include requirements for color and duration
    of error messages, physical placement of critical indicators or keys, and
    use of auditory signals.

..

.. _trainingreqs:

Training-related requirements
-----------------------------

.. todo::

    This paragraph shall specify the CSCI requirements, if any, pertaining to
    training. Examples include training software to be included in the CSCI.

..

.. _logisticsreqs:

Logistics-related requirements
------------------------------

.. todo::

    This paragraph shall specify the CSCI requirements, if any, concerned with
    logistics considerations. These considerations may include: system
    maintenance, software support, system transportation modes, supply system
    requirements, impact on existing facilities, and impact on existing
    equipment.

..

.. _otherreqs:

Other requirements
------------------

.. _exportcontrol:

Export control
~~~~~~~~~~~~~~

The software produced under this contract is subject to export control restrictions
on encryption components. Any software libraries, or encryption keys, *must* be
acquired or produced by the end user implementing DIMS, *not* distributed as part
of the DIMS code base. The plan is to release software components with instructions on
how to acquire and install the necessary cryptographic elements *before* beginning the
installation process.


.. _packagingreqs:

Packaging requirements
----------------------

.. todo::

    This section shall specify the requirements, if any, for packaging,
    labeling, and handling the CSCI for delivery (for example, delivery on 8
    track magnetic tape labelled and packaged in a certain way).  Applicable
    military specifications and standards may be referenced if appropriate.

..

.. _noencryption:

No included cryptographic elements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Per Section :ref:`exportcontrol`, all software packaged for release *must* have
checks to confirm that cryptographic libraries and/or encrypt keys are
*not present* in the packaged source or delivered system component(s).


.. _precedenceofreqs:

Precedence and criticality of requirements
------------------------------------------

.. todo::

    This paragraph shall specify, if applicable, the order of precedence,
    criticality, or assigned weights indicating the relative importance of the
    requirements in this specification. Examples include identifying those
    requirements deemed critical to safety, to security, or to privacy for
    purposes of singling them out for special treatment. If all requirements
    have equal weight, this paragraph shall so state.

..

