---
tags:
- sentence-transformers
- sentence-similarity
- feature-extraction
- dense
- generated_from_trainer
- dataset_size:1599
- loss:CosineSimilarityLoss
base_model: sentence-transformers/all-MiniLM-L6-v2
widget:
- source_sentence: 'Hierarchical Task Networks (HTN) generate plans using a decomposition
    process guided by extra domain knowledge to guide search towards a planning task.
    While many HTN planners can make calls to external processes (e.g. to a simulator
    interface) during the decomposition process, this is a computationally expensive
    process, so planner implementations often use such calls in an ad-hoc way using
    very specialized domain knowledge to limit the number of calls. Conversely, the
    few classical planners that are capable of using external calls (often called
    semantic attachments) during planning do so in much more limited ways by generating
    a fixed number of ground operators at problem grounding time. In this paper we
    develop the notion of semantic attachments for HTN planning using semi co-routines,
    allowing such procedurally defined predicates to link the planning process to
    custom unifications outside of the planner. The ing planner can then use such
    co-routines as part of its backtracking mechanism to search through parallel dimensions
    of the state-space (e.g. through numeric variables). We show empirically that
    our planner outperforms the state-of-the-art numeric planners in a number of domains
    using minimal extra domain knowledge. Planning in domains that require numerical
    variables, for example, to drive robots in the physical world, must represent
    and search through a space defined by real-valued functions with a potentially
    infinite domain, range, or both. This type of numeric planning problem poses challenges
    in two ways. First, the description formalisms BID6 might not make it easy to
    express the numeric functions and its variables, ing in a description process
    that is time consuming and error-prone for real-world domains BID17. Second, the
    planners that try to solve such numeric problems must find efficient strategies
    to find solutions through this type of state-space. Previous work on formalisms
    for domains with numeric values developed the Semantic Attachment (SA) construct
    BID3 ) in classical planning. Semantic attachments were coined by (Weyhrauch 1981)
    to describe the attachment of an interpretation to a predicate symbol using an
    external procedure. Such construct allows the planner to reason about fluents
    where numeric values come from externally defined functions. In this paper, we
    extend the basic notion of semantic attachment for HTN planning by defining the
    semantics of the functions used as semantic attachments in a way that allows the
    HTN search and backtracking mechanism to be substantially more efficient. Our
    current approach focused on depth-first search HTN implementation without heuristic
    guidance, with free variables expected to be fullyground before task decomposition
    continues. Most planners are limited to purely symbolic operations, lacking structures
    to optimize usage of continuous resources involving numeric values BID9. Floating
    point numeric values, unlike discrete logical symbols, have an infinite domain
    and are harder to compare as one must consider rounding errors. One could overcome
    such errors with delta comparisons, but this solution becomes cumbersome as objects
    are represented by several numeric values which must be handled and compared as
    one, such as points or polygons. Planning descriptions usually simplify such complex
    objects to symbolic values (e.g. p25 or poly2) that are easier to compare. Detailed
    numeric values are ignored during planning or left to be decided later, which
    may force replanning BID17. Instead of simplifying the description or doing multiple
    comparisons in the description itself, our goal is to exploit external formalisms
    orthogonal to the symbolic description. To achieve that we build a mapping from
    symbols to objects generated as we query semantic attachments. Semantic attachments
    have already been used in classical planning BID3 ) to unify values just like
    predicates, and their main advantage is that new users do not need to discern
    between them and common predicates. Thus, we extend classical HTN planning algorithms
    and their formalism to support semantic attachment queries. While external function
    calls map to functions defined outside the HTN description, we implement SAs as
    semi co-routines BID1, subroutines that suspend and resume their state, to iterate
    across zero or more values provided one at a time by an external implementation,
    mitigating the potentially infinite range of the external function. Our contributions
    are threefold. First, we introduce SAs for HTN planning as a mechanism to describe
    and evaluate external predicates at execution time. Second, we introduce a symbol-object
    table to improve the readability of symbolic descriptions and the plans generated,
    while making it easier to handle external objects and structures. Finally, we
    empirically compare the ing HTN planner with a modern classical planner BID10
    in a number of mixed symbolic/numeric domains showing substantial gains in speed
    with minimal domain knowledge. Classical planning algorithms must find plans that
    transform properties of the world from an initial configuration to a goal configuration.
    Each property is a logical predicate, a tuple with a name and terms that relate
    to objects of the world. A world configuration is a set of such tuples, which
    is called a state. To modify a state one must apply an operator, which must fulfill
    certain predicates at the current state, as preconditions, to add and remove predicates,
    the effects. Each operator applied creates a new intermediate state. The set of
    predicates and operators represent the domain, while each group of objects, initial
    and goal states represent a problem within this domain. In order to achieve the
    goal state the operators are used as rules to determine in which order they can
    be applied based on their preconditions and effects. To generalize the operators
    and simplify description one can use free variables to be replaced by objects
    available, a process called grounding. Once a state that satisfies the goal is
    reached, the sequence of ground operators is the plan BID14. A plan is optimal,
    iff it achieves the best possible quality in some criteria, such as number of
    operators, time or effort to execute; or satisficing if it reaches the goal without
    optimizing any metrics. PDDL BID11 ) is the standard description language to describe
    domains and problems, with features added through requirements that must be supported
    by the planner. Among such features are numeric-valued fluents to express numeric
    assignments and updates to the domain, as well as events and processes to express
    effects that occur in parallel with the operators in a single instant or during
    a time interval. Hierarchical planning shifts the focus from goal states to tasks
    to exploit human knowledge about problem decomposition using a hierarchy of domain
    knowledge recipes as part of the domain description BID13 ). This hierarchy is
    composed of primitive tasks that map to operators and non-primitive tasks, which
    are further refined into sub-tasks using methods. The decomposition process is
    repeated until only primitive-tasks mapping to operators remain, which in the
    plan itself. The goal is implicitly achieved by the plan obtained from the decomposition
    process. If no decomposition is possible, the task fails and a new expansion is
    considered one level up in the hierarchy, until there are no more possible expansions
    for the root task, only then a task decomposition is considered unachievable.
    Unlike classical planning, hierarchical planning only considers tasks obtained
    from the decomposition process to solve the problem, which both limits the ability
    to solve problems and improves execution time by evaluating a smaller number of
    operators. The HTN planning description is more complex than equivalent classical
    planning descriptions, since it includes domain knowledge with potentially recursive
    tasks, being able to solve more problems than classical planning. Classical planners
    with heuristic functions can solve problems mixing symbolic and numeric values
    efficiently using a process of discretization. A discretization process converts
    continuous values into sets of discrete symbols at often predefined granularity
    levels that vary between different domains. However, if the discretization process
    is not possible, one must use a planner that also supports numeric features, which
    requires another heuristic function, description language and usually more computing
    power due to the number of states generated by numeric features. Numeric features
    are especially important in domains where one cannot discretize the representation,
    they usually appear in geometric or physics subproblems of a domain and cannot
    be avoided during planning. Unlike symbolic approaches where literals are compared
    for equality during precondition evaluation, numeric value comparison is non-trivial.
    To avoid doing such comparison for every numeric value the user is left responsible
    for explicitly defining when one must consider rounding errors, which impacts
    description time and complexity. For complex object instances (in the object-oriented
    programming sense), such as polygons that are made of point instances, comparison
    details in the description are error-prone. Details such as the order of polygon
    points and floating point errors in their coordinates are usually irrelevant for
    the planner and the domain designer and should not be part of the domain description
    as they are part of a lowlevel specification. Such low-level specifications can
    be implemented by external function calls to improve what can be expressed and
    computed by a HTN planner. Such functions come with disadvantages, as they are
    not expected to keep an external state, returning a single value solely based
    on the provided parameters. While HTN planners can abstract away the numeric details
    via external function calls, there are limitations to this approach if a particular
    function is used in a decomposition tree where it is expected to backtrack and
    try new values from the function call (i.e. if the function is meant to be used
    to generate multiple terms as part of the search strategy). An external function
    must return a list of values to account for all possible decompositions so the
    planner tries one at a time until one succeeds. Generating a complete list is
    too costly when compared to computing a single value, as the first value could
    be enough to find a feasible plan. A semantic attachment, on the other hand, acts
    as an external predicate that unifies with one possible set of values at a time,
    rather than storing a complete list of possible sets of values to be stored in
    the state structure. This implementation saves time and memory during planning,
    as only backtracking causes the external co-routine to resume generating new unifications
    until a plan (or a certain amount of plans) is found. Each SA acts as a black
    box that simulates part of the environment encoding the in state variables that
    are often orthogonal to other predicates BID8. While common predicates are stored
    in a state structure, SAs are computed at execution time by co-routines. With
    a state that is not only declarative, with parts being procedurally computed,
    it is possible to minimize memory usage and delegate complex state-based operations
    to external methods otherwise incompatible or too complex for current planning
    description languages and planners that require grounding. We abstract away the
    numeric parts of the planning process encoded through SAs in a layer between the
    symbolic planner and external libraries. We leverage the abstract architecture
    of FIG0 with three layers inspired by the work of de BID2. In the symbolic layer
    we manipulate an anchor symbol as a term, such as polygon1, while in the external
    layer we manipulate a Polygon instance with N points as a geometric object based
    on what the selected external library specifies. With this approach we avoid complex
    representations in the symbolic layer. Instances created by the external layer
    that must be exposed to the symbolic layer are compared with stored object instances
    to reuse a previously defined symbol or create a new one, i.e. always represent
    position 2,5 with p1. This process makes symbol comparison work in the planning
    layer even for symbols related to complex external objects. The symbol-object
    table is also used to transform symbols into usable object instances by external
    function calls and SAs. Such table is global and consistent during the planning
    process, as each unique symbol will map the same internal object, even if such
    symbol is discarded in one decomposition branch. Once operations are finished
    in the external layer the process happens in reverse order, objects are transformed
    back into symbols that are exposed by free variables. The intermediate layer acts
    as the foreign function interface between the two layers, and can be modified
    to accommodate another external library without modifications to the symbolic
    description. SAs can work as interpreted predicates BID12, evaluating the truth
    value of a predicate procedurally, and also grounding free variables. SAs are
    currently limited to be used as method preconditions, which must not contain disjunctions.
    As only conjunctions and negations are allowed, one can reorder the preconditions
    during the compilation phase to improve execution time, removing the burden of
    the domain designer to optimize a mostly declarative description by hand, based
    on how free variables are used as SA Consider the abstract method example of Listing
    1, with two SAs among preconditions, sa1 and sa2. The compiled output shown in
    Algorithm 1 has both SAs evaluated after common predicates, while function calls
    happen before or after each SA, based on which variables are ground at that point.
    In Line 4 the free variables fv1 and fv3 have a ground value that can only be
    read and not modified by other predicates or SAs. In Line 7 every variable is
    ground and the second function call can be evaluated. Algorithm 1 Compilation
    phase may reorder preconditions to optimize execution time. DISPLAYFORM0 for each
    fv1, fv3; state ⊂ {pre1,t1,t2, pre2,fv3,fv1} do 4:for each sa1(t1, fv1) do 5:
    free variable fv2 6:for each sa2(fv1, fv2) do 7: DISPLAYFORM1 The other limitation
    of current SA co-routines is that they must unify with a valid value within their
    internal iterations or have a stop condition, otherwise the HTN process will keep
    backtracking and evaluating the SA seeking new values and never returning failure.
    Due to the implementation support of arbitrary-precision arithmetic and accessing
    data from real-world streams of data/events (which are always new and potentially
    infinite) a valid value may never be found, and we expect the domain designer
    to implement mechanisms to limit the maximum number of times a SA might try to
    evaluate a call (i.e. to have finite stop conditions). This maximum number of
    tries can be implemented as a counter in the internal state of a SA, which is
    mostly used to mark values provided to the HTN to avoid repetition, but may achieve
    side-effects in external structures. The amount of side-effects in both external
    functions calls and SAs increase the complexity of correctness proofs and the
    ability to inspect and debug domain descriptions. A common problem when moving
    in dynamic and continuous environments is to check for object collisions, as agents
    and objects do not move across tiles in a grid. One solution is to calculate the
    distance between both objects centroid positions and verify if this value is in
    a safe margin before considering which action to take. To avoid the many geometric
    elements involved in this process we can map centroid position symbols to coordinate
    instances and only check the symbol returned from the symbol-object table, ignoring
    specific numeric details and comparing a symbol to verify if objects are near
    enough to collide. This process is illustrated in Figure 2, in which p 0 and p
    1 are centroid position symbols that match symbols S 0 and S 1 in the symbol-object
    table, which maps their value to point objects O 0 and O 1. Such internal objects
    are used to compute distance and return a symbolic distance in situations where
    the actual numeric value is unnecessary. Figure 2: The symbol to object table
    maps symbols to object-oriented programming instances to hide procedural logic
    from the symbolic layer. In order to find a correct number to match a spatial
    or temporal constraint one may want to describe the relevant interval and precision
    to limit the amount of possibilities without having to discretely add each value
    to the state. Planning descriptions usually do not contain information about numeric
    intervals and precision, and if there is a way to add such information it is through
    the planner itself, as global definitions applied to all numeric functions, i.e.
    timestep, mantissa and exponent digits of DiNo BID15 ). The STEP SA described
    in Algorithm 2 addresses this problem, unifying t with one number at time inside
    the given interval with an step. To avoid having complex effects in the move operators
    one must not update adjacencies between planning objects during the planning process.
    Instead one must update only the Algorithm 2 The STEP SA replaces the pointer
    of t with a numeric symbol before resuming control to the HTN.1: function STEP(t,
    min = 0, max = ∞, = 1) 2: for i ← min to max step do 3: t ← symbol(i) 4:yield
    Resume HTN object position, deleting from the old position and adding the new
    position. Such positions come from a partitioned space, previously defined by
    the user. The positions and their adjacencies are either used to generate and
    store ground operators or stored as part of the state. To avoid both one could
    implement adjacency as a co-routine while hiding numeric properties of objects,
    such as position. Algorithm 3 shows the main two cases that appear in planning
    descriptions. In the first case both symbols are ground, and the co-routine resumes
    when both objects are adjacent, doing nothing otherwise, failing the precondition.
    In the second case s2, the second symbol, is free to be unified using s1, the
    first symbol, and a set of directions D to yield new positions to replace s2 pointer
    with a valid position, one at a time. In other terms, this co-routine either checks
    whether s2 is adjacent to s1 or tries to find a value adjacent to s1 binding it
    to s2 if such value exists. Algorithm 3 This ADJACENT SA implementation can either
    check if two symbols map to adjacent positions or generate new positions and their
    symbols to unify s2. DISPLAYFORM0 yield 8: else if s2 is free 9:for each (x, y)
    ∈ D do 10:nx ← x + x(s1); ny ← y + y(s1) 11:if 0 ≤ nx < WIDTH ∧ 0 ≤ ny < HEIGHT
    12:s2 ← symbol(nx, ny) 13: yield We conducted emprirical tests in a machine with
    Dual 6-core Xeon CPUs @2GHz / 48GB memory, repeating experiments three times to
    obtain an average. The show a substantial speedup over the original classical
    description from ENHSP BID16 ) with more complex descriptions. Our HTN implementation
    is available at github.com/Maumagnaguagno/HyperTensioN U. In the Plant Watering
    domain BID7 one or more agents move in a 2D grid-based scenario to reach taps
    to obtain certain amounts of water and pour water in plants spread across the
    grid. Each agent can carry up to a certain amount of water and each plant requires
    a certain amount of water to be poured. Many state variables can be represented
    as numeric fluents, such as the coordinates of each agent, tap and plant, the
    amount of water to be poured and being carried by each agent, and the limits of
    how much water can be carried and the size of the grid. There are two common problems
    in this scenario, the first is to travel to either a tap or a plant, the second
    is the top level strategy. To avoid considering multiple paths in the decomposition
    process one can try to move straight to the goal first, and only to the goal in
    scenarios without obstacles, which simplifies the travel method. To achieve this
    straightforward movement we modify the ADJACENT SA to consider the goal position
    also, using an implementation of Algorithm 4. The top level strategy may consider
    which plant is closer to a tap or closer to an agent, how much water an agent
    can carry and so on. The simpler top level strategy is to verify how much water
    must be poured to a plant, travel to a tap, load water, travel to the previously
    selected plant and pour all the water loaded. Repeating this process until every
    plant has enough water poured. The travel method description using our modified
    JSHOP input language is shown in Listing 2 and compiled to Algorithm 5. We compare
    with the fastest satisficing configurations of ENHSP (sat and c sat) in FIG1,
    which shows that our approach is faster with execution times constantly below
    0.01s, with both planners obtaining nonstep-optimal plans. Algorithm 4 In this
    goal-driven ADJACENT SA the positions are coordinate pairs, and two variables
    must be unified to a closer to the goal position in an obstacle-free scenario.1:
    function ADJACENT(x, y, nx, ny, gx, gy) 2: x ← numeric(x); y ← numeric(y) 3: gx
    ← numeric(gx); gy ← numeric(gy) 4:compare returns -1, 0, 1 for <, =, >, respectively
    5: nx ← symbol(x + compare(gx, x)) 6: ny ← symbol(y + compare(gy, y)) 7: yield
    In the Car Linear domain BID0 the goal is to control the acceleration of a car,
    which has a minimum and maximum speed, without external forces applied, only moving
    through one axis to reach its destination, and requiring a small speed to safely
    stop. The idea is to propagate process effects to state functions, in this case
    acceleration to speed and speed to position, while being constrained to an acceptable
    speed and acceleration. The planner must decide when and for how long to increase
    or decrease acceleration, therefore becoming a temporal planning problem. We use
    a STEP SA to iterate over the time variable and propagate temporal effects and
    constraints, i.e. speed at time t. We compare the execution time of our approach
    with ENHSP with aibr, ENHSP main configuration for planning with autonomous processes,
    in TAB4. There is no comparison with a native HTN approach, as one would have
    to add a discrete finite set of time predicates (e.g. time 0) to the initial state
    description to be selected as time points during planning. For an agent to move
    in a continuous space it is common practice to simplify the environment to simpler
    geometric shapes for faster collision evaluation. One possible simplification
    is to find a circle or sphere that contains each obstacle and use this new shape
    to evaluate paths. In this context the best path is the one with the shortest
    lines between initial position and goal, considering bitangent lines between each
    simplified obstacle plus the amount of arc traversed on their borders, also know
    as Dubins path BID5. One possible approach for a satisficing plan is to move straight
    to the goal or to the closest obstacle to the goal and repeat the process. A precondition
    to such movement is to have a visible target, without any other obstacle between
    the current and target positions. A second consideration is the entrance direction,
    as clock or counterclockwise, to avoid cusped edges. Cusped edges are not part
    of optimal realistic paths, as the moving agent would have to turn around over
    a single point instead of changing its direction a few degrees to either side.
    For the problem defined in FIG3 Two possible approaches can be taken to solve
    the search over circular obstacles using bitangents. One is to rely on an external
    solver to compute the entire path, a motion planner, which could happen during
    or after HTN decomposition has taken place. When done during HTN decomposition,
    as seen in Listing 3, one must call the SEARCH-CIRCULAR function and consume the
    ing steps of the plan stored in the intermediate layer, not knowing about how
    close to the goal it could reach in case of failure. When done after HTN decomposition,
    one must replace certain dummy operators of the HTN plan and replan in case of
    failure. The second approach is to rely on parts of the external search, namely
    the VIS-IBLE function and CLOSEST SA, to describe continuous search to the HTN
    planner. The VISIBLE function returns true if from a point on a circle one can
    see the goal, false otherwise. The CLOSEST SA generates unifications from a circle
    with an entrance direction to a point in another circle with an exit direction,
    new points closer to the goal are generated first. Differently from external search,
    one can deal with failure at any moment, while being able to modify behavior with
    the same external parts, such as the initial direction the search starts with.
    Another advantage over the original solution is the ability to ask for N plans,
    which forces the HTN to backtrack after each plan is found and explore a different
    path until the amount of plans found equals N or the HTN planner fails to backtrack.
    A description of such approach is show in Listing 4. The execution time variance
    between the solutions is not as important as their different approaches to obtain
    a , from an external greedy best-first search to a HTN depth-first search. The
    external search also computes bitangents on demand, as bitangent precomputation
    takes a significant amount of time for many obstacles. We developed a notion of
    semantic attachments for HTN planners that not only allows a domain expert to
    easily define external numerical functions for real-world domains, but also provides
    substantial improvements on planning speed over comparable classical planning
    approaches. The use of semantic attachments improves the planning speed as one
    can express a potentially infinite state representation with procedures that can
    be exploited by a strategy described as HTN tasks. As only semantic attachments
    present in the path decomposed during planning are evaluated, a smaller amount
    of time is required when compared with approaches that precompute every possible
    value during operator grounding. Our description language is arguably more readable
    than the commonly used strategy of developing a domain specific planner with customized
    heuristics. Specifically, we allow designers to easily define external functions
    in a way that is readable within the domain knowledge encoded in HTN methods at
    design time, and also dynamically generate symbolic representations of external
    values at planning time, which makes generated plans easier to understand. Our
    work is the first attempt at defining the syntax and operation of semantic attachments
    for HTNs, allowing further research on search in SA-enabled domains within HTN
    planners. Future work includes implementing a cache to reuse previous values from
    external procedures applied to similar previous states BID4 ) and a generic construction
    to access such values in the symbolic layer, to obtain data from explored branches
    outside the state structure, i.e. to hold mutually exclusive predicate information.
    We plan to develop more domains, with varying levels of domain knowledge and SA
    usage, to obtain better comparison with other planners and their ing plan quality.
    The advantage of being able to exploit external implementations conflicts with
    the ability to incorporate such domain knowledge into heuristic functions, as
    such knowledge is outside the description. Further work is required to expose
    possible metrics from a SA to heuristic functions.'
  sentences:
  - This paper proves the universal  approximability of quantized ReLU neural networks
    and puts forward the complexity bound given arbitrary error.
  - 'We present a theoretically proven generative model of knowledge graph embedding. '
  - An approach to perform HTN planning using external procedures to evaluate predicates
    at runtime (semantic attachments).
- source_sentence: 'We were approached by a group of healthcare providers who are
    involved in the care of chronic patients looking for potential technologies to
    facilitate the process of reviewing patient-generated data during clinical visits.
    Aiming at understanding the healthcare providers'' attitudes towards reviewing
    patient-generated data, we conducted a focus group with a mixed group of healthcare
    providers. Next, to gain the patients'' perspectives, we interviewed eight chronic
    patients, collected a sample of their data and designed a series of visualizations
    representing patient data we collected. Last, we sought feedback on the visualization
    designs from healthcare providers who requested this exploration. We found four
    factors shaping patient-generated data: data & context, patient''s motivation,
    patient''s time commitment, and patient''s support circle. Informed by the of
    our studies, we discussed the importance of designing patient-generated visualizations
    for individuals by considering both patient and healthcare provider rather than
    designing with the purpose of generalization and provided guidelines for designing
    future patient-generated data visualizations. Collecting patient-generated data
    is becoming increasingly common in chronic disease management. Patients use technological
    tracking tools to collect health and lifestyle data in disparate places. Both
    healthcare providers and patients agree that this data could be used to make smarter
    decisions to improve patients'' quality of life and to aid providers in making
    decisions about patient ongoing care. There are already technological tools for
    tracking and visualizing health data such as sleep (e.g., ), physical activity
    (e.g., ), variations in weight (e.g., ), and blood sugar level (e.g., ). However,
    most of these tracking tools are not designed to fully meet patients and healthcare
    providers'' expectations and do not support reviewing patient-generated data with
    healthcare providers during clinical visits. One way to support patients in presenting
    their data with healthcare providers is to visualize the patient-generated data
    collections effectively. Yet, we lack an understanding of what type of visualization
    designs can support chronic patients to present and review their health data with
    healthcare providers during clinical visits. To answer this question, we explored
    patients'' and healthcare providers'' perspectives on presenting and reviewing
    patient data. To extract healthcare provider requirements when reviewing patientgenerated
    data during a clinical visit, we conducted a focus group with a mixed group of
    healthcare providers. To uncover patient stories and their approaches to tracking
    and presenting their health data, we interviewed eight patients with chronic conditions
    who actively track their health data. Our findings revealed four factors shaping
    patient-generated data: data items & data context collected by patients, time
    commitment invested by patients to track data, patients'' motivation for collecting
    data, and patients'' support circle. Considering these four factors, we designed
    various visualizations representing patient-generated data collections we gathered
    from our patients. Instead of pursuing a single generalized visualization design,
    we designed individually tailored visualizations for each patient. Based on our
    preliminary visualization designs, we proposed a design space of patient-generated
    data visualizations. Next, using these individually tailored visualization designs
    as elicitation artifacts, we interviewed the healthcare providers who had initiated
    the request for this project to reflect on the designs. Healthcare providers pointed
    to four use cases that they envision these visualizations could support their
    practice. As a whole, the of all our studies led to one message: the importance
    of designing patient-generated data visualizations by considering each patient
    and healthcare provider rather than designing for generalization. However, it
    may seem impossible to either design a unique set of visualizations for each patient
    or expect patients to design their own visualizations. We, as healthcare technology
    designers, need to provide patients and providers with a set of visualization
    designs as starting points. This approach would let each patient and provider
    choose the visualization designs that work the best for them with the capacity
    to customize the designs based on their lifestyle, conditions, collected data,
    and patientprovider relationships. Our contributions in this paper are as follow:
    We identified four factors shaping patient-generated data. We presented a design
    space of visualizations representing patient-generated data collections. We provided
    guidelines for designing future patient-generated data visualizations. In the
    this section, first we discuss patients'' perspectives and goals for collecting
    their health data. In the second part, we provide an overview of healthcare providers''
    perspectives on the benefits and the challenges of using patient-generated data
    in their practice. In the last part, we discuss how technological and visualization
    tools can support patients and healthcare providers with presenting and reviewing
    patient-generated data collections. The number of patients with chronic conditions
    is increasing every day in the world. The nature of chronic conditions requires
    close monitoring and self-managing care for these patients. A survey study in
    2013 showed at least seven out of ten adults in the U.S. track a health indicator
    for themselves or for someone whom they take care. An increase in the availability
    of wearable sensors, mobile health apps, and novel portable technologies provided
    patients with an extra boost to track more personal health data. People track
    their health data in various forms, including memorization, original artifacts,
    personal paper records, personal electronic records, and electronic patient portals.
    Patients track different types and amount of data depending on their personal
    health goals. These goals can range from preventing more complications, having
    more control over their health, setting personal health goals, improving their
    conditions, and sharing these self-collected data with their healthcare providers.
    Studies have shown that sharing patient-generated data with healthcare providers
    can improve patient-provider communication. Sharing health data also empowers
    patients in taking control of the conversation during a clinical visit and helps
    healthcare providers build a relationship with patients. Many patients share their
    self-collected health data with their healthcare providers during clinical visits
    seeking tailored medical advice. Some healthcare providers see value in patients
    collecting their health data and presenting them during clinical visits. They
    think that by reviewing patient-generated data, they will gain more insights into
    patient goals and will be able to provide more tailored care to patients. Providers
    think, in some cases, patientgenerated data might be more reliable than clinic
    measurements because the data is collected at more frequent intervals, and there
    is less recall bias. Providers mentioned that often, a hospital''s electronic
    medical record system have misinformation or inaccuracies. In addition, patient
    data measured in the clinic (such as blood pressure) may be affected by the white
    coat effect and stress of the clinical environment. In these situations, patient-generated
    data can be used to reconcile these inaccuracies as patientgenerated data may
    contain less false-positive data than patient health data collected in the clinic.
    We should note that although healthcare providers may find patient-generated data
    complementary to clinical measurements and history taking if tracked in a meaningful
    way, they do not consider this data as a replacement to clinically measured data.
    Patients may not be willing to record their data when they have abnormal readings
    due to fear of consequences and may be worried that their data will be part of
    their permanent clinical record. In addition, providers sometimes express frustrations
    when patients do not track enough data, track excessive data, or track non-meaningful
    data. Patients also use different mediums and organization formats that work best
    for them to collect and present their health data. As a , the patient-generated
    data collections become heavily personal and complex, making it challenging for
    healthcare providers to understand and analyze. It is difficult to find the time
    to examine unrequested data during a short clinical visit. Most clinical visits
    are currently short. The common clinical visits with family physicians usually
    last about 10 to 20 minutes, leaving a short amount of time for reviewing patient-generated
    data. The providers may not find as much value reviewing patient-generated data
    during a clinical visit. Storing this data safely and securely can be challenging
    for providers and can add to their workload. Thus, there is still not a fully
    clear understanding of how, when, and what type of patient-generated data is most
    useful to review and discuss during clinical visits. One way to facilitate reviewing
    patient-generated data would be to have standardized data collection and presentation
    processes. However, a standardized process is probably not a panacea, as every
    patient and healthcare provider may have individualized preferences and needs.
    There is evidence that technology can support providers and patients in improving
    the quality of communicating patient data. Previous work raised questions about
    how technology should be designed that could assist both patients and healthcare
    providers in smoothly reviewing patient-generated data during clinical visits.
    One way could be visualizing these patient-generated data. Visualizing this data
    can benefit both patients and providers, if carefully designed so that it seamlessly
    integrates both perspectives into patient care planning. However, designing a
    general solution that works for all patients and providers is not easily achievable.
    Thus, first we need to move towards designing tailored visualizations, making
    an individualized visualization experience for each patient and provider. We were
    approached by a group of healthcare providers from a local hospital who are involved
    in the care of chronic patients to explore if, and how, to design technology that
    can enhance the process of presenting and reviewing patient-generated data during
    a clinical visit. To answer this question, we took an iterative design approach
    with close involvement of both patients and healthcare providers. The Institutional
    Review Board of (anonymized) University approved this study. First, we conducted
    a focus group with the healthcare provider that voiced concerns for reviewing
    patient-generated data. Then, to complete the healthcare providers'' perspectives
    reviewing patientgenerated data,, we interviewed eight patients actively collect
    health data and collected a sample of their data. We asked our patient participants
    about their experience collecting, analyzing, and sharing their data with healthcare
    providers. Next, we leveraged this understanding to propose potential visualization
    designs representing patient-generated data that we collected. Our goal was to
    design visualizations to improve the process of reviewing these data during clinical
    visits. Last, we interviewed healthcare providers seeking their reflection on
    our proposed visualization designs. We also asked the providers how they envision
    using these visualizations in their practice. To clarify, confirm, and gain a
    deeper understanding of the healthcare providers'' perspectives about the patient-generated
    data collection review process, we conducted a formal focus group with a mixed
    group of healthcare providers. Our focus group included a subgroup of providers
    who initially approached us including a clinical endocrinologist (with 21 years
    of experience), one internal medicine specialist physician (with 29 years of experience),
    and one healthcare provider (with 9 years of experience) directly supporting patients
    who monitor their data. Three other healthcare researchers were present during
    the focus group listening to the discussions. In our focus group, we asked healthcare
    providers about their experiences reviewing the patient-generated data, analyzing
    and understanding the patient data, and giving advice to patients based on their
    data. One interviewer primarily posed the questions during the discussion and
    two other researchers from our interview team took field notes. The focus group
    lasted around 60 minutes. We video-recorded, transcribed the focus group discussions,
    and later we used the grounded theory to analysis the data. To understand patients''
    perspectives on tracking and presenting their self-generated health data, we interviewed
    eight patients who suffer from one or multiple chronic conditions. We used several
    methods of recruitment for this study: emails from a local Patient Care Networks
    directors, Patient Care Networks newsletter ads, targeted recruitment through
    the healthcare provider who participated in the focus group and snowball sampling.
    We conducted an hour long semi-structured interview with each patient. We formed
    our patient interview questions based on the of our discussions during the focus
    group with healthcare providers. We asked participants to bring a sample of their
    data to the interview session and walk us through their data sample in detail.
    We video-recorded and transcribed all the interviews. To analyze the interview
    , we used the grounded theory method, analyzing each interview in a separate process.
    Our goal was to reach a deeper understanding of each patient''s story. We state
    proof of existence for each interview and do not try to generalize our findings
    across patients. Next, based on the requirements of each individual patient, we
    sketched various visualization alternatives representing their own patient-generated
    data collections. As a group, we discussed the visualizations and how they meet
    the patients'' needs. Then, we selected one or several alternative designs that
    best matched the patient''s requirements. To complete our design cycle, we took
    our visualization designs back to three healthcare providers, who were among the
    group that initiated this project, seeking their feedback. We interviewed an internal
    medicine physician with 29 years of experience (C1), a clinical endocrinologist
    with 21 years of experience (C2), and a complex chronic specialist physician with
    22 years of experience (C3). Each session lasted between 40-60 minutes and was
    video recorded and later transcribed. In the interview session, we first gave
    the providers a description of the patients'' conditions, their personal stories,
    and their data collection processes. Then, we shared the visualization designs
    with the providers and observed their reactions walking through and talking out
    loud about the designs. From our analysis of the focus group transcripts, we extracted
    four requirements by our healthcare provider participants, to support reviewing
    patient-generated data during clinical visits. R1-Integrating data context: Healthcare
    providers think patient sometimes collect too many data items, but data without
    context is not helpful for medical purposes, "you get the data in a 7 by 6 table
    with numbers and they are all over the place. Without food information, stress
    information, activity information it does look like a bunch of noise. You don''t
    see a pattern without being able to query on those other dimensions. Like your
    sugar is high, are you stressed?" (C1). To overcome this challenge, providers
    need tools that are able to integrate context with data. R2-Summarizing for quick
    presentation of data: Patients sometimes come to clinical visits with a large
    collection of data and expect their healthcare providers to help them make sense
    of their data "they clearly put in a lot of work, but you don''t have time and
    you have nowhere to begin" (C1). Healthcare providers want tools with abilities
    to summarize and filter patient data to see trends, patterns, and anomalies. R3-Sharing
    goals and motivations: Our healthcare providers told us patients usually have
    different goals than providers which may cause conflicts. Patients often like
    to discuss details of their data, but providers are more interested in an overview
    of the whole data, so they wanted "a platform that forces people to be explicit
    between stakeholders" (C2). With this in mind, providers wanted to have tools
    with ability to overview and focus on parts of the data to explore the patient
    data in both focused and detailed views accommodating their goals and patients''
    goals. R4-Supporting conversations: Both patients and healthcare providers need
    support to discuss their concerns "[patient says] I have questions about [this]
    and the doctor says ok, great, that is what is going on there. But I am more concerned
    about this" (C1). Healthcare providers told us they need support opening up communications
    with patients which may have not happened otherwise; tools that can represent
    patient data in different views letting patients and providers discuss various
    aspects of patient data. The findings from the focus group helped us form our
    patient interview questions. Our healthcare providers found patient-generated
    data useful when patients collect meaningful data with context. Thus, in our patient
    interviews, we asked our patient participants to talk about the data items and
    the context data they collect. Our providers expressed their concerns about patients
    committing an excessive amount of time on data collection ing in large datasets.
    Thus, to get patient perspectives in this manner, we asked our patient participants
    to tell us about their time commitment to data collection. Our healthcare providers
    talked about the impact of patient goals and motivation on their data collection
    and data sharing. Thus, in our patient interviews, we asked patients to tell us
    about their goals and motivation for collecting data and if they were advised
    to track data by their providers. Our healthcare providers saw value in having
    a patient''s presence during clinical conversations. Thus, we asked the patients
    whether they shared their data with their healthcare providers or their caregivers
    at home and how was their experience getting support. To design the patient-generated
    visualization designs, we considered the four requirements (R1-R4) identified
    from our focus group and followed the design guidelines established in the literature.
    To accommodate data context integration, R1, in the visualization, we used "Tooltip"
    which is an identifying tool presenting the attribute data attached to an object.
    To incorporate R2, we followed the basic information seeking principles. To fulfill
    R3, we incorporated "overview and details-on-demand" interactions in our designs.
    To support patients and providers view patient data from different perspectives,
    R4, we designed multiple visualization designs for each patient-generated data
    collection. We allocated pseudonyms to confer the anonymity of our patients. In
    each part of this section, we first present the profile of the patient; their
    data and context, their time commitment, their motivation, and their support circle.
    For each patient, we ideated and designed one or multiple visualizations. These
    visualization designs are simple visualizations that are carefully designed to
    capture providers'' requirements and each individual patient needs and may not
    be novel designs by themselves. We explain the detail of each visualization design
    we sketched to display patient data and how we took the providers'' and patients''
    requirements into considerations when exploring visualization design opportunities
    to represent their patient-generated data collections. We did not restrict ourself
    to designing a certain number of visualization representations; we sketched as
    many design possibilities as we could think of to present the data for the patient.
    In total, we generated 20 preliminary visualization designs for eight patients.
    We laid out these designs on a design space board (Fig. 1). In this design space,
    each column corresponds to one patient and the visualizations in the column are
    design variations for that patient. Later, as a group, we discussed all of the
    visualization designs and selected the designs that best represent each individual
    patient. In this figure, the selected designs are highlighted with an orange border.
    We acknowledge that these designs are not the only possible visualizations and
    other designers/researchers may come up with variations to these designs. Here,
    we present our designs and we hope this will be a starting point for other researchers
    and designers to contribute more patient stories to the literature and to move
    towards thinking about designing more for individuals. Maria is 67 years old,
    one day she experienced high blood pressure and visited the hospital emergency
    room. After that hospital visit, Maria constantly experienced high blood pressure.
    That year, she was diagnosed with hypertension. Data & context: Maria was advised
    to track her blood pressure and heart rate on a regular basis using a cuff machine.
    She uses a notebook to record her readings (Fig. 3 -a). We designed a visualization
    representing both Maria''s blood pressure and heart rate readings (Fig. 1 -column
    p#1 -first row). We display blood pressure readings in the form of bars and show
    the patient''s heart rate on demand. Each bar represents one blood pressure reading,
    we associate the bottom border of the bar to diastolic and the top border of the
    bar to systolic. The two horizontal lines in the show the normal blood pressure
    reading range (120 over 80). In addition, we added colour to each bar showing
    a normal (green), an abnormal (yellow), or a dangerous (red) blood pressure reading.
    Time commitment: Maria tracks her blood pressure and heart rate three to four
    times per day. Thus in our design each bar in the visualization shows one reading
    with the time of the recording. Motivation: Maria''s ultimate goal for tracking
    her data is "to feel better... make my blood pressure go down" (P01). After her
    diagnosis, she changed her life style to reach her goals. She is drinking more
    fluids and reduced the amount of salt in her diet. She is hopeful that she can
    reach her goal. She also keeps a record of events or activities she thinks may
    be relevant to her blood pressure, so later during a medical visit, she can discuss
    them with her healthcare providers. Thus, in our design we have an option to add
    notes associated with her blood pressure records. Support circle: Maria presents
    her notebook to her family physician saying, "because of this [notebook], it will
    be easier for me to inform the doctor" (P01). She hopes her family physician can
    make sense of the data and make adjustments to her treatment plans based on her
    data. To accommodate Maria''s need for sharing her data, we designed this visualization
    with the capacity to show an overview of blood pressure readings over months (top
    row in the design) to quickly check her overall status in the past months as well
    as detailed numbers on demands (bottom row in the design). Andrew was diagnosed
    with type 1 diabetes about 16 years ago at the age of 52. Due to his age, he was
    first misdiagnosed with type 2 diabetes. After his diagnosis, his interaction
    with the healthcare system changed from visiting his family physicians once a
    year to getting an A1C test every three months. He has been in direct interaction
    with a nurse educator, a foot care clinic, and an endocrinologist in a diabetic
    neuropathology clinic. Data & context: Andrew measures his blood glucose and basal
    rate as advised by his nurse educator and endocrinologist (Fig. 3 -b). He uses
    a glucose meter to measure the concentration of glucose in his blood and an insulin
    pump to calculate the amount of insulin required. We represent Andrew''s blood
    glucose data in two different visualization designs. The (Fig. 1 -column p#2 -first
    row) is a detailed view of one day of Andrew''s glucose level. The circle shows
    a 24-hour clock. Each time Andrew measures his glucose, we show his reading on
    that time on the clock with a bar. The height of the bar represents the glucose
    rate and the color of the bar represents the normality of the glucose rate; if
    the glucose reading is too low (red), low (yellow), normal (green), high (yellow),
    or too high (red). In the (Fig. 1 -column p#2 -second row), the top part shows
    all blood glucose ratings recorded in a month with circular points. The y-axis
    shows the glucose rate and the x-axis shows the date. We also double coded each
    data point with the same colour themes as the first design. Time commitment: Before
    each meal, Andrew measures his blood glucose using the glucose meter and enters
    his readings into the insulin pump. The pump automatically send Andrew''s insulin
    intake to his nurse educator. Besides that, Andrew keeps track of his basal rates
    that he measures using the glucose meter, in a notebook to later share with his
    nurse educator. Every time Andrew visits his healthcare providers to check on
    his conditions, he shares the recorded data he collected over the past few months
    with his healthcare providers. Thus, in our visualization designs, we included
    a weekly or monthly overview of his glucose rates at the bottom of both designs.
    Motivation: Andrew lives a good life, eats healthy, gets enough sleep, and has
    a balanced work-life lifestyle. He recently got diabetes complications. After
    experiencing the complications, he is hoping to start an exercise routine. Andrew
    tracks his exercise on the side to understand the effect of his physical activities
    on his blood glucose. Thus, we added an option for the patient to add a free style
    note (e.g., exercise) on his data point to appear on demand when hovering over
    the data point in the visualizations. Support circle: Andrew has a hard time analyzing
    and finding trends in his data to adjust his lifestyle saying, " There''s so many
    factors that come to play with your blood sugars and trying to get everything
    in the right spot" (P02). He expects his healthcare providers to make sense of
    his data for him and give him direct instructions on how to better manage his
    conditions. Thus, we included a weekly and a monthly view of the glucose recordings
    on the bottom of both designs to give an overview of his data. Jen is 34 years
    old and was diagnosed with hypertension when she was 18 years old and was medicated
    for a few months. Last year, she had a visit with her family physician to get
    treatment for an infection and her blood pressure reading was high at the clinic.
    But, when she checked her blood pressure at home, she noticed that her reading
    was closer to normal readings than in the clinic. Data & context: Jen tracks her
    blood pressure and heart rate (Fig. 3 -c). Since she is experiencing a steady
    heart rate, she mainly focuses on her blood pressure data. Thus, we only display
    her blood pressure data, in two different visualization alternatives. We designed
    two visualizations displaying Jen''s blood pressure data. In the (Fig. 1 -column
    p#3 -first row) design, we have designed a tree based visualization with the ability
    to expand on demand. The top root represent the average blood pressure readings
    of the patient over one year. The next level shows the seasons, then months, and
    lastly the daily blood pressure reading. Jen uses three different colors to distinguish
    her readings into normal, borderline, and abnormal. With colour coding her numbers,
    she can quickly glance over her data. We have used the same idea in our visualization
    design and color coded her blood pressure readings. In the (Fig. 1 -column p#3
    -second row) design, each bar shows an average of all Jen''s blood pressure readings
    in a day, where the colours indicate the normality of the number. Dark green indicates
    high blood pressure readings, green indicates a normal blood pressure readings,
    and light green indicates low blood pressure readings. Looking at this view, she
    can decide if she is having more dark or light colors in a period of time. Whenever
    she decides to focus on a certain period of time, she can select that section
    and a table view appears underneath with data displayed for each day. Time commitment:
    Jen has been measuring her blood pressure a few times per week for a year and
    believes her condition is under control with steady normal blood pressure readings:
    "Lately, it''s been quite good for the last several months. So, kind of since
    January I check it maybe once a week now as opposed to every day"(P03). Thus,
    in our designs we only display maximum one reading per day. Motivation: Since
    her last clinical visit, Jen monitors her numbers to prevent any complications
    or developing hypertension for the second time. Last time she was taking medications
    for her hypertension, she experienced many side effects, and she fears that the
    healthcare providers may medicate her again: "I''ve been borderline and they''ve
    talked about medicating me for it, but I would rather not be if I can avoid it.
    So, I am just trying to manage it other ways before getting to that point" (P03).
    Support circle: Jen usually does light exercises, gardening, or short walks to
    stay healthy. To stay under 1500 mg sodium per day she plans her weekly meals
    with her husband. She expressed her concerns to her physicians that she only has
    high blood pressure when she is at the clinic, visiting her providers gets her
    anxious and stressed. To overcome this problem, she writes notes next to her readings
    keeping track of any triggering factors such as a clinical visit. She is hoping
    by showing the numbers she tracked at home to her healthcare providers, she can
    tell them, "No, it''s usually right around 120/80. It''s not always this high"
    (P03). Therefore, in our designs, we have an option for Jen to mark the blood
    pressure readings measured during her clinical visits. Lucas is 43 years old and
    suffers from hypertension, type 2 diabetes, and depression. Lucas was hospitalized
    a few times with suicidal thoughts and high blood glucose. Tracking his blood
    pressure and glucose level helps him get his conditions under control; however,
    sometimes he experiences an emotional break down when his readings are higher
    than the normal range advised by his providers. Data & context: Lucas collects
    his glucose, blood pressure, and heart rate (Fig. 3 -d) in his notebooks. We designed
    two visualizations displaying all three items he is tracking (Fig. 1 -column P#4).
    Lucas wants to look at his glucose, blood pressure, and heart rate data all at
    once. Thus, we display all his data in one view. Each data point is color coded
    in both visualizations based on the ranges defined for Lucas''s conditions. Green
    indicates normal, yellow shows borderline, and the out of range readings are colored
    in red. Time commitment: Lucas was advised by his providers to record his data
    five times a day. However, he is dealing with a lot of pressure due to his conditions
    and his personal problems, so he only manages to track his data once a day. Thus,
    in the (Fig. 1 column P#4 -first row), each vertical division in the chart shows
    one data item: blood pressure, blood glucose, and heart rate. In the (Fig. 1 -column
    P#4 -second row), we show each day of data readings in a flower shape visualization,
    each petal representing one data item: blood pressure, blood glucose, and heart
    rate. Motivation: He feels frustrated and upset with himself for not having his
    conditions under control. Lucas hopes to get support that motivates him to track
    his data, but does not want to be pushed. He wants to exercise regularly, as it
    can help him stabilize his blood pressure and glucose level; however, his busy
    schedule does not allow for exercise. Instead, he tries to go for short walks
    to lower his blood pressure when he experiences high blood pressure. His goal
    is to get off the insulin by next year. Support circle: Lucas feels that he does
    not have enough family support and his family lacks compassion and doesn''t understand
    the seriousness of his conditions. He has difficulty making sense of his data
    and expects his healthcare providers to understand his data and give him advice
    based on them. For instance, he was hoping to find relations between his blood
    pressure readings and glucose level, but could not find any correlations. Thus,
    we visualized all the three data items he collects adjacent to each other in one
    view. Ken is 37 years old and suffers from multiple conditions. He had memory
    problems, paranoia, and learning difficulties since childhood. He was diagnosed
    with behavioural disorder in 2005, mental health problems in 2009, and asperger
    syndrome in 2011. In addition, Ken has digestive problems and is experiencing
    pain in different parts of his body (e.g., neck, back, shoulder, ankle), which
    have not been officially diagnosed. Data & context: Ken tracks his nutrition data
    and symptoms related to his stomach pain and bowel movements using MySymptoms
    app. He tracks his pain to help with diagnosing the source of his pain (Fig. 3
    -e). To understand the effects of his mental state on his conditions, he also
    tracks his mood. Ken prefers using multiple apps on his tablet to record different
    health data items, therefore we also visualized his data in two separate designs.
    He is happy with the app he uses for tracking his nutrition, so we focused our
    designs on the other data items (moon and pain). We sketched a visualization displaying
    Ken''s mood data (Fig. 1 -column P#5 -first row). Each day on the calendar shows
    Ken''s mood of the day which is colour coded; happy (green), normal (yellow),
    sad (red), and selfdefined (blue). We sketched a second visualization representing
    Ken''s pain data (Fig. 1 -column P#5 -second row). Time commitment: Ken tracks
    his mood once everyday. To present his mood data, in the calendar visualization,
    we also allow for one mood entry per day (Fig. 1 -column P#5 -first row). On the
    other hand, the pain body mock-up visualization (Fig. 1 column P#5 -second row)
    does not display any time data and lets Ken record as many occurrences as he experiences.
    Each ring in this visualization represents pain experienced once in the marked
    location of body. Motivation: Ken''s goals are to eat healthier, get more physically
    active, and lose weight. Also, he is hoping to get more involved in his care.
    He records relevant context as notes that he thinks may trigger his mood. Thus,
    we added an option for him to add free style notes to keep track of the context
    associated with each day in the calendar view visualization. Support circle: Ken
    tracks several symptoms and trigger factors that he thinks may be helpful for
    improving his health, but his healthcare providers do not always find his collected
    data useful. He is confused about which data items are useful to collect: "I gave
    all my symptoms to her, all recorded on a sheet. She said,''Oh, we''re just looking
    at the gut issues.'' I''m like, What about the rest?" (P05). Sarah is 49 years
    old and was diagnosed with type 1 diabetes in 1984. In 2013, Sarah was hospitalized
    experiencing severe gastroparesis symptoms. Later, Sarah developed arthritis in
    her hand and gets cortisone shots, which increases her glucose level after each
    shot. Data & context: She uses an insulin pump to manage her diabetes (Fig. 3
    -f). Thus, in our visualizations, we represent her blood glucose data (Fig. 1
    -column P#6 -first row). Since Sarah has an insulin pump, the device automatically
    tracks her blood glucose many times in a day. Thus, to visually show all the data
    points measured by her insulin pump in a day, we designed a clock visualization.
    The clock view can show all the data readings in one view with their timestamp.
    Sarah''s healthcare providers predefined a normal range of glucose level for her
    based on her conditions. In the (Fig. 1 -column P#6 -first row) visualization,
    the blood glucose reading is marked with an X inside each ring and colour coded
    to green, yellow, and red based on the ranges defined for her. Time commitment:
    The pump automatically tracks her blood glucose level in different time intervals
    to program her insulin. Sarah does not regularly record her food intake, but when
    she feels sick, she takes notes in her phone of what she ate and her activities
    that may have affected her glucose: "there''s really no answer, I''ve been dealing
    with this for about two or three years now " (P06). Motivation: Sarah has changed
    her lifestyle especially after her diagnosis with gastroparesis. She takes an
    active role in managing her conditions. She says "with gastroparesis there''s
    no medication, there''s no cure... it''s a matter of just doing a lot of research
    and reading in different avenues (P06). Sarah has a fear of getting sick to the
    extent that she needs hospitalization. Support circle: Her diabetes nurse monitors
    Sarah''s glucose level regularly. On the occasion that Sarah feels sick or in
    need of help, she calls her nurse and asks her nurse to log into her pump remotely.
    Based on her pump , the nurse will give her advice on how to normalize her glucose
    level. To discuss her readings over a week with her nurse, we displayed an overview
    in form of seven rings (days) (Fig. 1 -column P#6 -first row). Tim is 56 years
    old and was diagnosed with type 2 diabetes about 8-10 years ago. His condition
    has gotten worse in the past two years. Tim has been also dealing with hypertension
    for a long time. He also has a genetic disorder, Hereditary Hemorrhagic Telangiectasia
    that cause abnormality in blood vessel formation, but it does not affect his chronic
    conditions. Data & context: Tim uses a glucose meter to measure his glucose reading
    and records his readings in an app on his phone (Fig. 3 -g). He also uses a blood
    pressure cuff machine to measure his blood pressure. He then manually enters his
    blood pressure readings into two different apps on his phone since he is afraid
    one app will wipe out his recorded data. He prefers to collect his data on his
    phone rather than the booklet he was given by the nurse. We display both data
    items in one design (Fig. 1 -column P#7). Tim takes notes keeping track of events
    and special occasions (i.e., holidays and parties). Thus, to accommodate recording
    these notes, we added an option in our design to track and later display the notes.
    Time commitment: He tracks his blood glucose once or twice a day and measures
    his blood pressure a few times a day at different times. Thus, we also show multiple
    data readings on the chart per day. Tim normally skips tracking his data during
    vacation times. However, not tracking his data during his last vacation caused
    an abnormality in his data: "I was good for a while. Then took a vacation and,
    whoaa!"(P07). To visually display the effect of not tracking data we show the
    missing dates with dashed lines. Motivation: After visiting a new physician, the
    physician changed Tim''s hypertension medication to a more recently developed
    medication. Since the change of his medication, his blood pressure has been generally
    stable and he got motivated to start tracking it, "I kicked myself, I should have
    tracked it longer"(P07). He is hoping to become more active in his care. Tim has
    a standing order from his diabetes nurse to get A1C test every three months. He
    is hoping his glucose level goes below 6.5: "six months ago, it was 8.1. Now it''s
    7.1"(P07). To make it easier for him to check if his numbers are normal, we colour
    coded (green, yellow, red) the data points. Support circle: Tim''s diabetes nurse
    and his family physician automatically receive the of his A1C test. However, Tim
    does not share any of his self-collected data with his providers. Katy is 52 years
    old and she suffers from hypertension, asthma, arthritis, chronic pain, and depression.
    She was diagnosed with asthma 21 years ago which is mostly under control with
    medications. In 2004, she gave birth to a premature baby and had a sudden death
    in her family. Later that year, she was diagnosed with severe depression and was
    hospitalized in the psychiatric ward. Data & context: As a of Katy''s depression,
    she gained 150 pounds. Three years ago, she joined a weight management group and
    was advised by her dietitian to track her food intake (Fig. 3 -h), but Katy does
    not like to share her collected data with her providers. A few years ago, Katy
    started to experience pain in certain areas of her upper body; however, her physician
    did not believe her pain was real and was dismissive to her condition. After struggling
    with pain for a while, she decided to look for another pain specialist. She created
    an Excel sheet with upper body part names and each day she would put in a number
    corresponding to her pain level in addition to the type of pain (stabbing, stinging,
    and shooting). Thus, we designed an upper body mock-up drawing visualization to
    help her visually track the type and location of her pain (Fig. 1 -column P#8
    -second row). In the visualization, the tensity of the pain is represented by
    the number of rings (1 to 10) and the three types of pain that Katy tracks are
    distinguished with different colours. Time commitment: Every time Katy experiences
    pain, she records her pain data. Thus, we also allow for as many (pain) data entry
    as pain occur during a day in our visualization design. Motivation: Katy writes
    side notes to her pain data to investigate if there is any relationship between
    the time of the day, her activities, and her pain level. She shared her pain diary
    with her new pain specialist to see if there is any relationship between the time
    of the day, her activities, and her pain level; Katy told us her specialist said:
    "This is great, there is no relationship to anything which just tells me it is
    probably a nerve or something. This is fabulous and I want to keep this!" (P08).
    Support circle: Katy hoped to receive more tailored care by sharing her self-collected
    health data with her healthcare providers. She sees value in tracking her health
    data and sharing them with her healthcare providers. We display an overview to
    her pain data by displaying a week of her pain data in form of small body mockups
    at the bottom of our design. This view will help providers to get an overview
    and to find possible patterns or trigger factors. We presented the patient-generated
    data visualizations to three of the providers who initially requested visualizations
    and technological support for reviewing patient-generated data. We observed providers''
    reactions towards our visualization designs and asked for their feedback. The
    providers varied widely in why, when, and how they want to use patient-generated
    data visualizations in their practices. We present our according to the two themes
    we identified through analyzing the interview data. The themes are 1) the visualizations''
    use cases in providers'' practice, and 2) the platforms for implementing the visualizations.
    Providers envisioned different use cases for the visualizations in their practice:
    1) one provider saw value in the use of these visualizations only by patients,
    2) two provider wanted to use them to review patient data during clinical visits
    collaboratively, 3) one provider thought of using them to support their medical
    judgment, and 4) two providers found displaying patient data through different
    lenses useful to understand the data better. Encourage patient self-experimentation
    and goal setting: The complex chronic care specialist, C3, expected visualization
    views that would encourage patients to do more self-experiments. He thinks particularly
    for chronic symptom management where there is no complete treatment to resolve
    the symptoms, but rather it is a matter of trying to track and manage them, experimenting
    to find trigger factors can be helpful for these patients. The potential of self-experimenting
    with data can help patients find solutions to perform easier through their everyday
    life. In addition, C3 thinks visualization designs need to have the capacity to
    support patients in setting goals and tracking an intervention that patients may
    set in their minds to control their symptoms.: " For example, taking three glasses
    of water per day may reduce headache" (C3). Although this provider was interested
    in encouraging patients to do selfexperiments and set goals, C3 wanted patients
    to share the data collections with them. In these circumstances, the providers
    can help patients understand if there is a scientific correlation between variables
    and help patients understand the body mechanism that might explain this correlation.
    Juxtapose data for collaborative interpretation: The internal medicine specialist,
    C1, was cautious about juxtaposing all patient-generated data items in a single
    visualization view. He was concerned that juxtaposing patient data could imply
    a link that may not exist and falsely medicalize the relation between the data:
    " the minute you put them on a shared data exhibit, it is a correlation" (C1).
    Although he was not enthusiastic about presenting some data items such as blood
    pressure and glucose level in one view, he found coupling some data points useful.
    For instance, when seeing the (Fig. 1 -column p#6 -first row) visualization he
    was keen to view patient food intake and blood sugars displayed together to investigate
    their relationship. Another functionality that the providers found useful was
    the potential to overlay data collected across different situations or days. By
    overlapping patient data, providers may be able to find patterns in patients''
    data. For instance, C3 was interested in overlapping patients'' glucose data over
    a few days to find out the effect of biking for 30 minutes on patient glucose
    levels, "the nature of the adjustments is very rarely a single day" (C3). Offer
    a holistic overview to the provider: The endocrinologist clinician, C2, showed
    interest in a holistic visualization view of all the data items a patient collects.
    She found the visualization designs that represented all patient data items in
    one view very useful for planning complex chronic patient care. For example, for
    displaying blood pressure and glucose level in one view she said: " as a care
    provider, I can show that''yeah, during these times these situations are really
    bad for you'' " (C2). She was also keen to see patient''s physical activities
    such as steps taken per day presented in the same view to understand the effect
    of exercise on the patient''s other health conditions. C2 was interested in having
    access to the patients'' notes describing the context and the situation when this
    data was recorded. She told us that she encourages her patients to take notes
    of their emotional states, their meals, or any other relevant information when
    recording their health data. Knowing the context associated with the data, the
    provider has more information to make informed medical judgments. Understand data
    better through a different lense: Providers were able to quickly adapt to new
    visualization designs and warmed up to the idea of alternative views of data,
    promising for adoption in their practice. For example, C3, said he appreciates
    the visualizations capability to display patient data differently. He said both
    patients and providers are used to seeing patient data in a standard tabular format.
    He thought showing patient data in different forms will give patients extra support
    in understanding their data and taking actions towards enhancing their health:
    " We never had this kind of things [visualizations] and so, this is where the
    notion of''same data, different lens'' becomes useful" (C3). The providers also
    recognized that some of these visualization designs can be used to represent other
    health measurements. One of the providers who was at first skeptical of using
    the blood pressure tree design (Fig. 1 -column p#3 -first row), after reviewing
    and discussing the design, suggested using this visualization for collectively
    displaying 24-hour blood pressure cuff machine data. Providers use these machines
    to closely monitor the patient blood pressure to help with diagnosis: "this is
    an attractive idea, maybe this kind of visualization can be used for a 24 hour
    report [showing data] every 10 minutes" (C1). The choice of visualization platform
    can make a difference in designing the right patient-generated data visualization.
    The providers talked to us about their preferred patient-generated data platforms,
    and the rationales, benefits, and trade-offs of their choices. Different technology
    and platforms for implementing such visualizations include data booklets, websites,
    phone apps, and patient portals. Booklets: To smoothly integrate visualizations
    into providers'' practices, one challenge is to design the patient-generated data
    visualizations compatible and aligned with the current providers'' practices.
    Providers usually give patients tabular template booklets to record data. C3 mentioned
    that he preferred reading patient data in these booklet format, since it is easier
    and faster for him to find trends. Printed visualizations in the form of booklets
    can be familiar and easy to use for providers, but do not support interactivity.
    Websites: Some providers prefer to have patient data uploaded on designated websites
    where providers could potentially integrate patient-generated data into the patient''s
    health records. C2 thought that, if designed well, a website would be a good platform
    that could support both patients and providers to interact with patient-generated
    data and see the data in different ways. However, healthcare services usually
    have restrictive policies for use of websites in clinical settings. Phone Apps:
    Patients may not feel comfortable sharing all the data with one healthcare provider
    and may only be willing to share related data with a specific provider depending
    on their specialty. C2 thought that using a personal phone to record data could
    be a solution, since patients have full authority to share data they wish. However,
    small display real estate could cause limitations in designing visualizations
    that represent all patient-generated data at once. Also, sharing a small display
    between patients and providers during clinical visits can be difficult. Patient
    Portals: Providers normally have a PC in their clinical rooms for taking specific
    notes about a patient''s condition and recording them in a patient''s healthcare
    portal. C1 was keen on the idea of asking patients to link their self-collected
    data into their healthcare portals ahead of time. He thought that having patientgenerated
    data collections and visualizations available on the portal could not only save
    time, but could also be easily accessible for discussion. However, implementing
    visualizations into these portals can be a long and difficult process, which also
    requires support from healthcare services. Effective communication of patient-generated
    data during clinical visits can help patients feel understood and support healthcare
    providers get all the necessary data they need to make proper medical decisions.
    Our objective was to design visualizations to support patients present patient-generated
    data for reviewing during clinical visits. The focus of our studies was on studying
    patients with chronic conditions and the healthcare providers who visit chronic
    patients. The of our patient interview studies revealed the individualities and
    the complexities of patient-generated data collections. Each patient has a unique
    body, a highly individualized lifestyle, a different set of goals, and a personalized
    patient-provider relationship. All these factors need to be considered while caring
    or designing for patients. How can we design only one visualization solution that
    can consider all these differences in patients? Providers also differed in their
    principle goal of using patientgenerated data. This has major implications on
    the design of visualization. A solution that works for one provider may not work
    for another. This may affect the types of visualizations we consider for them
    and their patients. There are many driving forces for designing effective patient-generated
    data visualizations. It is still unclear which direction works best for both patients
    and providers. In software and technology design, research, and businesses, there
    is often the notion of designing with a generalization mindset,''one-size-fits-all''.
    The idea of designing one software or one visualization tool that can address
    everyone''s problem may be appealing and cost-efficient, but it does not always
    bring validation. We echo the call from recent research for the necessity to design
    for particulars, individuals. Looking into medical literature and the approaches
    taken in healthcare services for patient care planning, we often see one-toone
    interactions between a patient and their healthcare providers in clinical visits.
    This one-to-one interaction model has been practiced for centuries in medicine
    and is tailored depending on the individualities of each patient and their healthcare
    provider. Similarly, for designing visualizations to improve patient-provider
    communication, we, as visualization and technology designers, should take directions
    from the medical literature and their practices. We should take steps towards
    designing individualized tailored visualizations based on both patient and provider
    preferences to be able to accommodate as many patient-provider communications
    as possible. Perhaps one solution can be to start designing and developing many
    patient-generated visualizations tailored based on both the healthcare provider
    and the patient preferences. There have been attempts in the literature to design
    visualizations representing patient-generated data for some chronic conditions
    including, visualizing bipolar patient lived experience, collaborative interactive
    storyboards design with chronic pediatric patients, photo-based visualization
    to support patients with irritable bowel syndrome communicate with providers.
    However, designing visualizations to support chronic patients with their self-collected
    data is indeterminant, or in other word a wicked problem, meaning there are no
    definitive solutions or limits to this design problem. Our design study in this
    paper is a step towards tackling this wicked problem. We followed two criteria
    required for a rigor design study when addressing a wicked problem: 1) A rigor
    solution to a wicked problem needs to include multiple perspectives to shape the
    design problem and consider a broad set of solutions. Thus, in our study, we included
    the perspectives of both patients and healthcare providers. We explored the healthcare
    provider perspectives on reviewing patient-generated data during clinical visits
    and the details of eight patients'' approaches to tracking and presenting their
    health data. Furthermore, we looked into a sample of our patient participants
    data collections to understand patients'' methods of recording their data and
    their reasoning. 2) A design study is not and should not be reproducible; rather
    the solutions proposed are one of many possible solutions. However, a rigor solution
    to a wicked problem needs to report the process of design and analysis in a transparent
    way. We designed multiple alternative visualizations for each patient. All of
    our visualizations together shaped a design space of variant patient-generated
    data representations. We understand that depending on the patient needs, the providers''
    expectations, and the patient-provider relationship dynamics, a different set
    of visualization designs could be suitable. Our solutions are one of the many
    possible solutions to this wicked problem. Furthermore, we explained the process
    of design and reflection of our design in detail transparently. We hope that the
    detailed design process we provided supports other researchers and designers to
    further tackle this wicked problem and to design patient-generated data visualizations.
    Inspired by the four factors identified from the focus group and the patient interviews,
    and the healthcare providers'' reflections on the designs, we provide the following
    design guidelines. We understand that these guidelines are not exhaustive; rather
    these are starting points to design patient-generated visualizations. Include
    data context in the design: Patients often track a large amount of data. To gain
    valuable insights from this data, the patients often take notes of the events,
    circumstances, or emotions associated with the data points. On the other hand,
    healthcare providers in our study found this contextual information useful to
    make medical decisions. Previous studies also pointed to the importance of relevant
    dimensions and context of data for making medical . Thus, visualization designs
    need to allow for smooth inclusion of the contextual data often in forms of free-format
    text along with the data points. Consider patients'' time commitments in the design:
    Chronic patients often deal with many issues in their everyday life, leaving them
    with less free time to track and record their data regularly. The Apps available
    on the market do not usually consider patient differences in the time they invest
    in collecting data. For instance, displaying empty entries can cause mental effects,
    making patients feel they are not doing enough. Thus, visualization designs should
    allow patients to customize the amount of information and input fields being shown.
    Allow patients to freely explore their data: Our show that the patient''s motivation
    for tracking and presenting their data to providers play an important role in
    the design. Some patients are eager to find correlations between their data, some
    patients are looking for causation of their symptoms, and some patients want to
    have an overview of their numbers. Previous work also stressed the need to support
    patients in sense-making and problem-solving with data. Thus, these differences
    in patients'' motivations for data collection should be considered when designing
    visualizations to represent patient-generated data. Support patients'' needs to
    (partially) share their data: Patients differ in the support they receive from
    their family, friends, and the healthcare team. Some patients benefit from sharing
    their whole data with their support circle, some are interested in sharing a selection
    of their data, and some are hesitant to share their data. Thus, visualization
    designs should support sharing overviews, selective views, and protected views.
    Visualization designs that support sharing views need to also include annotation
    capability and multiple views (e.g., patient view and clinician view). Support
    providers interacting with patient data: Although providers had different perspectives
    on the use cases of patientgenerated data visualizations in their practice, they
    had commonalities in regards to necessary interactive functionalities. All of
    our providers talked about how difficult it can be to cope with messy, inconsistent,
    and complicated data collections. This suggests that ata-glance data comprehension
    is an important visualization design goal. In addition, providers needed interactions
    to better understand the data including filtering the data, focusing into data
    details, and overlaying different parts of the data for comparison. In recent
    years, we have seen growing interests among patients with chronic conditions to
    track and analyze their data. However, sharing this data with healthcare providers
    can be challenging due to limited time in clinical visits and the large and complex
    nature of patient-generated data. We responded to a group of healthcare providers''
    call from a local hospital to design potential technological solutions to address
    the challenges of presenting, reviewing, and analyzing patient-generated data
    collections. We first gained healthcare providers'' perspectives through a focus
    group. Then, we took an in-depth look at chronically ill patients'' perspectives
    tracking their health data. The individual differences among these patients promoted
    a design space approach where we used insights from these patients to design a
    space of possible tailored visualizations. By exploring the possibilities of designing
    individual tailored visualizations representing patient-generated data, we have
    added one way that can support patients and healthcare providers when reviewing
    patient-generated data during clinical visits. We hope our proposed visualizations
    provide patients and healthcare providers better opportunities to present, review,
    and gain insights on patientgenerated data. We note that we included the perspectives
    of a small number of patients and healthcare providers; thus, other perspectives
    may not be included in our . However, we envision this study as a stepping stone
    for the call to focus more on designing technologies in healthcare for individuals.
    We encourage the human-computer interaction, visualization, and healthcare communities
    to repeat these studies by including more patients and healthcare providers and
    explore designing tailored visualizations for each individual. Then, as a community,
    we can move towards accumulating these perspectives and designs to empower individuals
    with accessible design variations. We hope that in the long term, the of this
    exploration contribute to supporting patients'' and healthcare providers'' in
    reviewing patient-generated data collections using visualizations during clinical
    visits.'
  sentences:
  - We explored the visualization designs that can support chronic patients to present
    and review their health data with healthcare providers during clinical visits.
  - We scale up lossless compression with latent variables, beating existing approaches
    on full-size ImageNet images.
  - Robust Bayesian Estimation via Maximum Mean Discrepancy
- source_sentence: 'Like humans, deep networks learn better when samples are organized
    and introduced in a meaningful order or curriculum. While conventional approaches
    to curriculum learning emphasize the difficulty of samples as the core incremental
    strategy, it forces networks to learn from small subsets of data while introducing
    pre-computation overheads. In this work, we propose Learning with Incremental
    Labels and Adaptive Compensation (LILAC), which introduces a novel approach to
    curriculum learning. LILAC emphasizes incrementally learning labels instead of
    incrementally learning difficult samples. It works in two distinct phases: first,
    in the incremental label introduction phase, we unmask ground-truth labels in
    fixed increments during training, to improve the starting point from which networks
    learn. In the adaptive compensation phase, we compensate for failed predictions
    by adaptively altering the target vector to a smoother distribution. We evaluate
    LILAC against the closest comparable methods in batch and curriculum learning
    and label smoothing, across three standard image benchmarks, CIFAR-10, CIFAR-100,
    and STL-10. We show that our method outperforms batch learning with higher mean
    recognition accuracy as well as lower standard deviation in performance consistently
    across all benchmarks. We further extend LILAC to state-of-the-art performance
    across CIFAR-10 using simple data augmentation while exhibiting label order invariance
    among other important properties. Deep networks have seen rich applications in
    high-dimensional problems characterized by a large number of labels and a high
    volume of samples. However, successfully training deep networks to solve problems
    under such conditions is mystifyingly hard . The go-to solution in most cases
    is Stochastic Gradient Descent with mini-batches (simple batch learning) and its
    derivatives. While offering a standardized solution, simple batch learning often
    fails to find solutions that are simultaneously stable, highly generalizable and
    scalable to large systems (; ; ;). This is a by-product of how mini-batches are
    constructed. For example, the uniform prior assumption over datasets emphasizes
    equal contributions from each data point regardless of the underlying distribution;
    small batch sizes help achieve more generalizable solutions, but do not scale
    as well to vast computational resources as large mini-batches. It is hard to construct
    a solution that is a perfect compromise between all cases. Two lines of work,
    curriculum learning and label smoothing, offer alternative strategies to improve
    learning in deep networks. Curriculum learning, inspired by strategies used for
    humans , works by gradually increasing the conceptual difficulty of samples used
    to train deep networks;; ). This has been shown to improve performance on corrupted
    and small datasets . More recently, deep networks have been used to categorize
    samples and variations on the pace with which these samples were shown to deep
    networks were analyzed in-depth . To the best of our knowledge, previous works
    assumed that samples cover a broad spectrum of difficulty and hence need to be
    categorized and presented in a specific order. This introduces computational overheads
    e.g. pre-computing the relative difficulty of samples, and also reduces the effective
    amount of data from which a model can learn in early epochs. Further, curriculum
    learning approaches have not been shown to compete with simple training strategies
    at the top end of performance in image benchmarks. A complementary approach to
    obtaining generalizable solutions is to avoid over-fitting or getting stuck in
    local minima. In this regard, label smoothing offers an important solution that
    is invariant to the underlying architecture. Early works like replace ground-truth
    labels with noise while uses other models'' outputs to prevent over-fitting. This
    idea was extended in to an iterative method which uses logits obtained from previously
    trained versions of the same deep network. use local distributional smoothness,
    based on the robustness of a model''s distribution around a data point, to regularize
    outcomes, penalized highly confident outputs directly. Closest in spirit to our
    work is the label smoothing method defined in , which offers an alternative target
    distribution for all training samples with no extra data augmentation. In general,
    label smoothing is applied to all examples regardless of how it affects the network''s
    understanding of them. Further, in methods which use other models to provide logits/labels,
    often the parent network used to provide those labels is trained using an alternate
    objective function or needs to be fully re-trained on the current dataset, both
    of which introduce additional computation. In this work, we propose LILAC, Learning
    with Incremental Labels and Adaptive Compensation, which emphasizes a label-based
    curriculum and adaptive compensation, to improve upon previous methods and obtain
    highly accurate and stable solutions. LILAC is conceived as a method to learn
    strong embeddings by using the recursive training strategy of incremental learning
    alongside the use of unlabelled/wrongly-labelled data as hard negative examples.
    It works in two key phases, 1) incremental label introduction and 2) adaptive
    compensation. In the first phase, we incrementally introduce groups of labels
    in the training process. Data, corresponding to labels not yet introduced to the
    model, use a single fake label selected from within the dataset. Once a network
    has been trained for a fixed number of epochs with this setup, an additional set
    of ground-truth labels is introduced to the network and the training process continues.
    In recursively revealing labels, LILAC allows the model sufficient time to develop
    a strong understanding of each class by contrasting against a large and diverse
    set of negative examples. Once all ground-truth labels are revealed the adaptive
    compensation phase of training is initiated. This phase mirrors conventional batch
    learning, except we adaptively replace the target one-hot vector of incorrectly
    classified samples with a softer distribution. Thus, we avoid adjusting labels
    across the entire dataset, like previous methods, while elevating the stability
    and average performance of the model. Further, instead of being pre-computed by
    an alternative model, these softer distributions are generated on-the-fly from
    the outputs of the model being trained. We apply LILAC to three standard image
    benchmarks and compare its performance to the strongest known baselines. While
    incremental and continual learning work on evolving data distributions with the
    addition of memory constraints ( and derivative works), knowledge distillation
    and similar works) or other requirements, this work is a departure into using
    negative mining and focused training to improve learning on a fully available
    dataset. In incremental/continual learning works, often the amount of data used
    to retrain the network is small compared to the original dataset while in LILAC
    we fully use the entire dataset, distinguished by Seen and Unseen labels. Thus,
    it avoids data deficient learning. Further, works like;; emphasize the importance
    of hard negative mining, both in size and diversity, in improving learning. Although
    the original formulation of negative mining was based on imbalanced data, recent
    object detection works have highlighted its importance in contrasting and improving
    learning in neural networks. To summarize, our main contributions in LILAC are
    as follows, • we introduce a new take on curriculum learning by incrementally
    learning labels as opposed to samples, • our method adaptively compensates incorrectly
    labelled samples by softening their target distribution which improves performance
    and removes external computational overheads, • we improve average recognition
    accuracy and decrease the standard deviation of performance across several image
    classification benchmarks compared to batch learning, a property not shared by
    other curriculum learning and label smoothing methods. In LILAC, our main focus
    is to induce better learning in deep networks. Instead of the conventional curriculum
    learning approach of ranking samples, we consider all samples equally beneficial.
    Early on, we focus on learning labels in fixed increments (Section 2.1). Once
    the network has had a chance to learn all the labels, we shift to regularizing
    the network to prevent over-fitting by providing a softer distribution as the
    target vector for previously misclassified samples (Section 2.2). An overview
    of the entire algorithm discussed is available in the appendix as Algorithm 1.
    In the incremental phase, we initially replace the ground-truth labels of several
    class using a constant held-out label. Gradually, over the course of several fixed
    intervals of training we reveal the true label. Within a fixed interval of training,
    we keep constant two sets of data, "Seen", whose groundtruth labels are known
    and "Unseen", whose labels are replaced by a fake value. When training, Illustration
    of the evolution of data partitions in the incremental label introduction phase
    for a four label dataset. In the first incremental step, only one label is used
    for training while the remaining data use label 4. A short period of training
    is performed with this fixed setup, where data from U is uniformly sampled to
    match the number of samples from S, in every mini-batch. The final incremental
    step depicted is equivalent to batch learning since all the labels are available
    to the network. Once all the ground-truth labels are revealed we begin the adaptive
    compensation phase described in Sec. 2.2. mini-batches are uniformly sampled from
    the entire training set, but the instances from "Unseen" classes use the held-out
    label. By the end of the final interval, we reveal all ground-truth labels. We
    now describe the incremental phase in more detail. At the beginning of the incremental
    label introduction phase, we virtually partition data into two mutually exclusive
    sets, S: Seen and U: Unseen, as shown in Fig. 1. Data samples in S use their ground-truth
    labels as target values while those in U use a designated unseen label, which
    is held constant throughout the entire training process. LILAC assumes a random
    ordering of labels, Or(M), where M denotes the total number of labels in the dataset.
    Within this ordering, the number of labels and corresponding data initially placed
    in S is defined by the variable b. The remaining labels, M − b, are initially
    placed in U and incrementally revealed in intervals of m labels, a hyper-parameter
    defined by the user. Training in the incremental phase happens at fixed intervals
    of E epochs each. Within a fixed interval, the virtual data partition is held
    constant. Every mini-batch of data is sampled uniformly from the entire original
    dataset and within each mini-batch, labels are obtained based on their placement
    in S or U. Then the number of samples from U is reduced or augmented, using a
    uniform prior, to match the number of samples from S. This is done to ensure no
    unfair skew in predictions towards U since all data points use the same designated
    label. Finally, the curated mini-batches of data are used to train the neural
    network. At the end of each fixed interval, we reveal another set of m groundtruth
    labels and move samples of those classes from U to S after which the entire data
    curation and training process is repeated for the next interval. Once all the
    ground-truth labels are available to the deep network, we begin the adaptive compensation
    phase of training. The main idea behind adaptive compensation is, if the network
    is unable to correctly predict a sample''s label even after allowing sufficient
    training time, then we alter the target vector to a less peaked distribution.
    Compared to learning one-hot vectors, this softer distribution can more easily
    be learned by the network. Unlike prior methods we adaptively modify the target
    vector only for incorrectly classified samples on-the-fly. In this phase, the
    network is trained for a small number of epochs using standard batch learning.
    Let T be the total number of training epochs in the incremental phase and batch
    learning. During the adaptive compensation phase, we start at epoch e, where e
    > T. For a mini-batch of samples in epoch e, predictions from the model at e −
    1 are used to determine the final target vector used in the objective function;
    specifically, we soften the target vector for an instance iff it was misclassified
    by the model at the end of epoch e − 1. The final target vector for the ith instance
    at epoch e, t e,i, is computed based on the model φ e−1 using Equation 1. Here,
    (x i, y i) denote a training sample and its corresponding ground-truth label for
    sample index i while δ yi represents the corresponding one-hot vector. 1 is a
    vector of M dimensions with all entries as 1 and is a scaling hyper-parameter.
    Datasets We use three datasets, CIFAR-10, CIFAR-100 and STL-10 , to evaluate our
    method and validate our claims. CIFAR-10 and CIFAR-100 are 10 and 100 class variants
    of the popular image benchmark CIFAR. Each of these contains 50,000 images in
    the training set and 10,000 images in the testing set. STL-10 is a 10 class subset
    of ImageNet with 500 and 800 samples per class for training and testing subsets,
    respectively. Metrics The common metric used to evaluate the performance of all
    the learning algorithms is average recognition accuracy(%) and standard deviation
    across 5 trials. We also report consistency, which is a binary metric that indicates
    whether the training strategy in higher average performance and lower standard
    deviation compared to standard batch learning across all datasets. Experimental
    Setup For CIFAR-10/100, we use ResNet18 as the architectural backbone for all
    methods; for STL-10, we use ResNet34. In each interval of LILAC''s incremental
    phase, we train the model for 10 epochs each for CIFAR-10/100, and 5 epochs each
    for STL-10. During these incremental steps, we use a learning rate of 0.1, 0.01
    and 0.1 for CIFAR-10, CIFAR-100, and STL-10 respectively. The standard batch learning
    settings used across all datasets are listed in the appendix. These settings reflect
    the setup used in LILAC once the incremental portion of training is complete and
    the algorithm moves into the adaptive compensation phase. Within this phase epochs
    175, 525 and 120 are used as thresholds (epoch T) for CIFAR-10, 100 and STL-10
    respectively. • Stochastic gradient descent with mini-batches is the baseline
    against which all methods are compared. • Curriculum learning ) forms a family
    of related works which aim to help models learn faster and optimize to a better
    minimum. Following the methodology proposed in this work we artificially create
    a subset of the dataset called "Simple", by selecting data that is within a value
    of 1.1 as predicted by a linear one-vs-all SVR model trained to regress to the
    ground-truth label. The deep network is trained on the "Simple" dataset for a
    fixed period of time that mirrors the total number of epochs of the incremental
    phase of LILAC after which the entire dataset is used to train the network. •
    Label Smoothing is the closest relevant work to use label smoothing as a form
    of regularization without extra data augmentation. This non-invasive baseline
    is used as a measure of the importance of regularization and for its ability to
    boost performance. • Dynamic Batch Size (DBS) is a custom baseline used to highlight
    the importance of variable batch size in training neural networks. DBS randomly
    copies data available within a mini-batch to mimic variable batch size. Further,
    all ground-truth labels are available to this model throughout the training process.
    • Random Augmentation (RA) is a custom baseline used to highlight the importance
    of virtual data partitions in LILAC. Its implementation closely follows LILAC
    but excludes the adaptive compensation phase. The main difference between LILAC
    and RA is that RA uses data from a one randomly chosen class, in U, within a mini-batch
    while data from all classes in U is used in LILAC to equalize the number of samples
    from S and U. Table 1 clearly illustrates improvements in average recognition
    accuracy, decrease in standard deviation and consistency when compared to batch
    learning. While certain setups have the highest 96.23 Fractional Max-pooling 96.53
    Densely Connected Convolutional Networks 96.54 Drop-Activation 96.55 Shake-Drop
    96.59 Shake-Drop + LILAC (ours) 96.79 performance on specific datasets (e.g.,
    Label Smoothing on CIFAR-10/100), they are not consistent across all datasets
    and do not find more stable solutions than LILAC (std. of 0.216 compared to 0.127
    from LILAC) LILAC is able to achieve superior performance without unnecessary
    overheads such as computing sample difficulty or irreversibly altering the ground-truth
    distribution across all samples. A key takeaway from DBS is the relative drop
    in standard deviation combined with higher average performance when compared to
    baselines like fixed curriculum and label smoothing. RA serves to highlight the
    importance of harvesting data from all classes in U simultaneously, for "negative"
    samples. The variety of data to learn from provides a boost in performance and
    standard deviation across the board in LILAC w/o AC as opposed to RA. DBS and
    RA underline the importance of variable batch size and data partitioning in the
    makeup of LILAC. We further extend LILAC to train the base pyramidnet with shake-drop
    regularization (p = 1.0) . From Table 2 we clearly see that LILAC can be extended
    to provide the highest performance on CIFAR-10 given a standard preprocessing
    setup. To provide a fair comparison we highlight top performing methods with standard
    preprocessing setups that avoid partial inputs (at the node or image level) since
    LILAC was developed with fully available inputs in mind. Across all these learning
    schemes, LILAC is the only one to consistently increase classification accuracy
    and decrease the standard deviation across all datasets compared to batch learning.
    Fig. 2 illustrates the evolution of the embedding across the span of the incremental
    phase. This space has more degrees of separation when compared to an equivalent
    epoch of training with batch learning where all the labels are available. Table
    3 provides a breakdown of the contribution of each phase of LILAC and how they
    combine to elevate the final performance. Here, in LILAC w/o AC we replace the
    entire AC phase with simple batch learning while in Batch + AC we include adaptive
    compensation with adjusted thresholds. The first half of this table compares the
    impact of incre- Figure 2: Side-by-side comparison between the representation
    spaces learned by LILAC and batch learning. Through the entire incremental label
    introduction phase, the representation space evolves to being more well spaced.
    Images in column 4 and 5 show comparable points in training space when all labels
    are available to the deep network being trained. These images support our claim
    that the deep network starts at a better initialization than standard batch learning,
    whose effect is carried throughout the training process. mentally introducing
    labels to a deep network against standard batch learning. We clearly observe that
    performances across Rows 1 and 2 fall within the indicated standard deviation
    of each other. However, from Fig. 2 we know that LILAC start from a qualitatively
    better solution. Combining these , we conclude that the emphasis on a lengthy
    secondary batch learning phase erodes overall performance. The second half of
    Table 3 shows the impact of adding adaptive compensation on batch learning and
    LILAC. When added to standard batch learning there isn''t a clear and conclusive
    indicator of improvement across all benchmarks in both average performance and
    standard deviation. However, in combination with the incremental label introduction
    phase of LILAC, adaptive compensation improves average performance as well as
    decreases standard deviation, indicating an improved stability and consistency.
    Given the similarity in learning between the batch setup and LILAC, when all labels
    have been introduced, we show that the embedding space learned by incrementally
    introducing labels (Fig. 2) is distinct from standard batch learning and is more
    amenable to AC. Through previous experiments we have established the general applicability
    of LILAC while contrasting its contributions to that of standard batch learning.
    In this section we dive deeper to reveal some characteristics of LILAC that further
    supplement the claim of general applicability. Specifically, we characterize the
    impact of label ordering, smoothness of alternate target vector distribution and
    injection of larger groups of labels in the incremental phase. Ordering of Labels
    Throughout the standard experiments, we assume labels are used in the ascending
    order of value. When this is modified to a random ordering or in ascending order
    of diffi- Table 4 suggest that there is no explicit benefit or pattern. Other
    than the extra impact of continually fluctuating label orders across trials, there
    isn''t a large gap in performance. Thus, we claim LILAC is relatively invariant
    to the order of label introduction. During adaptive compensation, = 0.5 is used
    in the alternate target vector for samples with failed predictions throughout
    all experiments in Sections 3.1 and 3.2. When extended to a variety of values,
    we observe that most variations of the peak performance still fall within the
    standard deviation range for each dataset. However, the peak average performance
    values usually occur between 0.7 to 0.5. While LILAC was designed to allow the
    introduction of multiple labels in a single incremental step, throughout the experiments
    in Sections 3.1 and 3.2 only 1 label was introduced per step to allow thorough
    learning while eliminating the chance of conflicting decision boundaries from
    multiple labels. Revealing multiple labels instead of 1 label per incremental
    step has a negative impact on the overall performance of the model. Table 4 shows
    that adding large groups of labels force lower performance, which is in line with
    our hypothesis that revealing fewer labels per incremental step makes the embedding
    space more amenable to adaptive compensation. In this work, we proposed LILAC
    which rethinks curriculum learning based on incrementally learning labels instead
    of samples. This approach helps kick-start the learning process from a substantially
    better starting point while making the learned embedding space amenable to adaptive
    negative logit compensation. Both these techniques combine well in LILAC to show
    the highest performance on CIFAR-10 for simple data augmentations while easily
    outperforming batch and curriculum learning and label smoothing on comparable
    network architectures. The next step in unlocking the full potential of this setup
    is to extend this setup to include a confidence measure on the predictions of
    network so that it can handle the effects of dropout or partial inputs. In further
    expanding LILAC''s ability to handle partial inputs, we aim to explore its effect
    on standard incremental learning (memory constrained) while also extending it
    applicability to more complex neural network architectures. A LILAC: ALGORITHM
    Table 8: The table captures the effect of varying the number of epochs used for
    the fixed training intervals in the incremental label introduction phase. Across
    CIFAR-10 there is an obvious peak after which the mean value decreases. However,
    in STL-10 there seems to be a consistent increase, with the assumption of minor
    noise. Finally, in CIFAR-100 there isn''t a clear pattern. From the in Table 8,
    we observe that the choice of E is dependent on the dataset. There isn''t an explicit
    pattern that can be used to select the value of E without trial runs. Further,
    the available run-time is an important constraint when select E from a range of
    values since both m and E affect it.'
  sentences:
  - A novel approach to curriculum learning by incrementally learning labels and adaptively
    smoothing labels for mis-classified samples which boost average performance and
    decreases standard deviation.
  - A novel, high-performing architecture for end-to-end named entity recognition
    and relation extraction that is fast to train.
  - alternative to gradient penalty
- source_sentence: 'In this paper, we explore new approaches to combining information
    encoded within the learned representations of autoencoders. We explore models
    that are capable of combining the attributes of multiple inputs such that a resynthesised
    output is trained to fool an adversarial discriminator for real versus synthesised
    data. Furthermore, we explore the use of such an architecture in the context of
    semi-supervised learning, where we learn a mixing function whose objective is
    to produce interpolations of hidden states, or masked combinations of latent representations
    that are consistent with a conditioned class label. We show quantitative and qualitative
    evidence that such a formulation is an interesting avenue of research. The autoencoder
    is a fundamental building block in unsupervised learning. Autoencoders are trained
    to reconstruct their inputs after being processed by two neural networks: an encoder
    which encodes the input to a high-level representation or bottleneck, and a decoder
    which performs the reconstruction using the representation as input. One primary
    goal of the autoencoder is to learn representations of the input data which are
    useful BID1, which may help in downstream tasks such as classification BID27 BID9
    or reinforcement learning BID20 BID5. The representations of autoencoders can
    be encouraged to contain more''useful'' information by restricting the size of
    the bottleneck, through the use of input noise (e.g., in denoising autoencoders,
    BID23, through regularisation of the encoder function BID17, or by introducing
    a prior BID11 . Another goal is in learning interpretable representations BID3
    BID10 . In unsupervised learning, learning often involves qualitative objectives
    on the representation itself, such as disentanglement of latent variables BID12
    or maximisation of mutual information BID3 BID0 BID8 .Mixup BID26 and manifold
    mixup BID21 are regularisation techniques that encourage deep neural networks
    to behave linearly between two data samples. These methods artificially augment
    the training set by producing random convex combinations between pairs of examples
    and their corresponding labels and training the network on these combinations.
    This has the effect of creating smoother decision boundaries, which can have a
    positive effect on generalisation performance. In BID21, the random convex combinations
    are computed in the hidden space of the network. This procedure can be viewed
    as using the high-level representation of the network to produce novel training
    examples and provides improvements over strong baselines in the supervised learning.
    Furthermore, BID22 propose a simple and efficient method for semi-supervised classification
    based on random convex combinations between unlabeled samples and their predicted
    labels. In this paper we explore the use of a wider class of mixing functions
    for unsupervised learning, mixing in the bottleneck layer of an autoencoder. These
    mixing functions could consist of continuous interpolations between latent vectors
    such as in BID21, to binary masking operations to even a deep neural network which
    learns the mixing operation. In order to ensure that the output of the decoder
    given the mixed representation resembles the data distribution at the pixel level,
    we leverage adversarial learning BID4, where here we train a discriminator to
    distinguish between decoded mixed and unmixed representations. This technique
    affords a model the ability to simulate novel data points (such as those corresponding
    to combinations of annotations not present in the training set). Furthermore,
    we explore our approach in the context of semi-supervised learning, where we learn
    a mixing function whose objective is to produce interpolations of hidden states
    consistent with a conditioned class label. Our method can be thought of as an
    extension of autoencoders that allows for sampling through mixing operations,
    such as continuous interpolations and masking operations. Variational autoencoders
    can also be thought of as a similar extension of autoencoders, using the outputs
    of the encoder as parameters for an approximate posterior q(z|x) which is matched
    to a prior distribution p(z) through the evidence lower bound objective (ELBO).
    At test time, new data points are sampled by passing samples from the prior, z
    ∼ p(z), through the decoder. In contrast, our we sample a random mixup operation
    between the representations of two inputs from the encoder. The Adversarially
    Constrained Autoencoder Interpolation (ACAI) method is another approach which
    involves sampling interpolations as part of an unsupervised objective BID2. ACAI
    uses a discriminator network to predict the mixing coefficient from the decoder
    output of the mixed representation, and the autoencoder tries to''fool'' the discriminator,
    making interpolated points indistinguishable from real ones. The GAIA algorithm
    BID18 ) uses a BEGAN framework with an additional interpolation-based adversarial
    objective. What primarily differentiates our work from theirs is that we perform
    an exploration into different kinds of mixing functions, including a semi-supervised
    variant which uses an MLP to produce mixes consistent with a class label. Let
    us consider an autoencoder model F (·), with the encoder part denoted as f (·)
    and the decoder g(·). In an autoencoder we wish to minimise the reconstruction,
    which is simply: DISPLAYFORM0 Because autoencoders trained by input-reconstruction
    loss tend to produce images which are slightly blurry, one can train an adversarial
    autoencoder BID14, but instead of putting the adversary on the bottleneck, we
    put it on the reconstruction, and the discriminator (denoted D) tries to distinguish
    between real and reconstructed x, and the autoencoder (which is analogous to the
    generator) tries to construct''realistic'' reconstructions so as to fool the discriminator.
    Because of this, we coin the term''ARAE'' (adversarial reconstruction autoencoder).
    This can be written as: DISPLAYFORM1 where GAN is a GAN-specific loss function.
    In our case, GAN is the binary cross-entropy loss, which corresponds to the Jenson-Shannon
    GAN BID4.One way to use the autoencoder to generate novel samples would be to
    encode two inputs h 1 = f (x 1) and h 2 = f (x 2) into their latent representation,
    perform some combination between them, and then run the through the decoder g(·).
    There are many ways one could combine the two latent representations, and we denote
    this function Mix(h 1, h 2). Manifold mixup BID21 implements mixing in the hidden
    space through convex combinations: DISPLAYFORM2 where λ ∈ (bs,) is sampled from
    a Uniform distribution and bs denotes the minibatch size. In contrast, here we
    explore a strategy in which we randomly retain some components of the hidden representation
    from h 1 and use the rest from h 2, and in this case we would randomly sample
    a binary mask m ∈ {0, 1} (bs×f) (where f denotes the number of feature maps) and
    perform the following operation: DISPLAYFORM3 where m is sampled from a Bernoulli(p)
    distribution (p can simply be sampled uniformly).With this in mind, we propose
    the adversarial mixup resynthesiser (AMR), where part of the autoencoder''s objective
    is to produce mixes which, when decoded, are indistinguishable from real images.
    The generator and the discriminator of AMR are trained by the following mixture
    of loss components: DISPLAYFORM4 fool D with reconstruction DISPLAYFORM5 fool
    D with mixes DISPLAYFORM6 label reconstruction as fake DISPLAYFORM7 label mixes
    as fake.Note that the mixing consistency loss is simply the reconstruction between
    the mixh mix = Mix(f (x), f (x)) and the re-encoding of it f (g(h mix)), where
    x and x are two randomly sampled images from the training set. This may be necessary
    as without it the decoder may simply output an image which is not semantically
    consistent with the two images which were mixed (refer to Section 5.2 for an in-depth
    explanation and analysis of this loss). Both the generator and discriminator are
    trained by the decoded image of the mix g(Mix(f (x), f (x))). The discriminator
    D is trained to label it as a fake image by minimising its probability and the
    generator F is trained to fool the discriminator by maximising its probability.
    Note that the coefficient λ controls the reconstruction and the coefficient β
    controls the mixing consistency in the generator. See Figure 1 for a visualisation
    of the AMR model. While it is interesting to generate new examples via random
    mixing strategies in the hidden states, we also explore a supervised mixing formulation
    in which we learn a mixing function that can produce mixes between two examples
    such that they are consistent with a particular class label. We make this possible
    by backpropagating through a classifier network p(y|x) which branches off the
    end of the discriminator, i.e., an auxiliary classifier GAN BID16.Let us assume
    that for some image x, we have a set of binary attributes y associated with it,
    where y ∈ {0, 1} k (and k ≥ 1). We introduce a mixing function Mix sup (h 1, h
    2, y), which is an MLP that maps y to Bernoulli parameters p ∈ bs×f. These parameters
    are used to sample a Bernoulli mask m ∼ Bernoulli(p) to produce a new combinationh
    mix = mh 1 + (1 − m)h 2, which is consistent with the class label y. Note that
    the conditioning class label should be semantically meaningful with respect to
    both of the conditioned hidden states. For example, if we''re producing mixes
    based on the gender attribute and both h 1 and h 2 are male, it would not make
    sense to condition on the''female'' label. To enforce this constraint, we simply
    make the conditioning label a convex combinationỹ mix = αy 1 + (1 − α)y 2 as well,
    using α ∼ Uniform. DISPLAYFORM0 The unsupervised version of the adversarial mixup
    resynthesiser (AMR). In addition to the autoencoder loss functions, we have a
    mixing function Mix which creates some combination between the latent variables
    h 1 and h 2, which is subsequently decoded into an image intended to be realistic-looking
    and semantically consistent with the two constituent images. This is achieved
    through the consistency loss (weighted by β) and the discriminator. To make this
    more concrete, the autoencoder and discriminator, in addition to their losses
    described in Equation 5, try to minimise the following losses: DISPLAYFORM1 label
    mixes as fake DISPLAYFORM2 Note that for the consistency loss the same coefficient
    β is used. See Figure 2 for a visualisation of the supervised AMR model. We use
    ResNets BID6 for both the generator and discriminator. The precise architectures
    for generator and discriminator can be found here.1 The datasets evaluated on
    are:• UT Zappos50K BID24: a large dataset comprising 50k images of shoes, sandals,
    slippers, and boots. Each shoe is centered on a white and in the same orientation,
    which makes it convenient for generative modelling purposes.• CelebA BID13: a
    large-scale and highly diverse face dataset consisting of 200K images. We use
    the aligned and cropped version of the dataset downscaled to 64px, and only consider
    (via the use of a keypoint-based heuristic) frontal faces. It is worth noting
    that despite this, there is still quite a bit of variation in terms of the size
    and position of the faces, which can make mixing between faces a more difficult
    task since the faces are not completely aligned.mixing with labels mixing without
    labels Figure 2: The supervised version of the adversarial mixup resynthesiser
    (AMR). The mixer function, denoted in this figure as Mix sup, takes h 1, h 2 and
    a convex combination of y 1 and y 2 (denotedỹ mix) and internally produces a Bernoulli
    mask which is then used to produce an output combinatioñ h mix = mh 1 + (1 − m)h
    2.h mix is then passed to the generator to generatex mix. In addition to fooling
    the discriminator usingx mix, the generator also has to make sure the class prediction
    by the auxiliary classifier is consistent with the mixed classỹ mix. Note that
    in this formulation, we still perform the kind of mixing which was shown in Figure
    1, and this is shown in the diagram with the component noted''mixing without labels''.
    BID2 and (d) the adversarial mixup resynthesiser (AMR). For more images, consult
    the appendix section. As seen in FIG0, all of the mixup variants produce more
    realistic-looking interpolations than in pixel space. Due to details in CelebA
    however, it is slightly harder to distinguish the quality between the different
    methods. Though this may not be the most ideal metric to use in our case (see
    discussion at end of this section) we use the Frechet Inception Distance (FID)
    by BID7, which is based on features extracted from a pre-trained CelebA classifier,
    to compute the distance between samples from the dataset and ones from our autoencoders
    2. Concretely, we compute (on the validation set) two scores: the FID between
    validation samples and their reconstructions (denoted in the table as FID(data,
    reconstruction)), and the FID between validation samples and randomly sampled
    interpolations (denoted in the table as FID(data, mix)). In the latter case, we
    repeat this five times (over five different sets of randomly sampled interpolations)
    for three different random seeds, ing in 5 × 3 = 15 FID scores from which we compute
    the mean and standard deviation. These are shown in TAB0 for the mixup and Bernoulli
    mixup formulations, respectively. Lower FID is usually considered to be better.
    However, FID may not be the most appropriate metric to use in our case. Because
    the FID is a measure of distance between two distributions, one can simply obtain
    a very low FID by simplying autoencoding the data, as shown in TAB0. In the case
    of mixing, one situation which may favour a lower FID is if g(αf ( DISPLAYFORM0
    ; in other words, the supposed mix simply decodes into one of the original examples
    x 1 or x 2, which clearly lie on the data manifold. To avoid having the mixed
    features αx 1 + (1 − α)x 2 being decoded back into samples which lie on the data
    manifold, we leverage the consistency loss, which is tuned by coefficient β. The
    lower the coefficient, the more likely that decoded mixes are projected back onto
    the manifold, but if this constraint is too weak then it may not necessarily be
    desirable if one wants to create novel data points. (For more details, see Section
    5.2 in the appendix.)Despite potential shortcomings of using FID, it seems reasonable
    to use such a metric to compare against baselines without any mixing losses, such
    as the adversarial reconstruction autoencoder (ARAE), which we indeed outperform
    for both mixup and Bernoulli mixup. For Bernoulli mixup, the FID scores appear
    to be higher than those in the mixup case TAB0 because the sampled Bernoulli mask
    m is also across the channel axis, i.e., it is of the shape (bs, f), whereas in
    mixup α has the shape (bs,). Because the mixing is performed on an extra axis,
    this produces a greater degree of variability in the mixes, and we have observed
    similar FID scores to the Bernoulli mixup case by evaluating on a variant of mixup
    where the α has the shape (bs, f) instead of (bs,). We present some qualitative
    with the supervised formulation. We train our supervised AMR variant using a subset
    of the attributes in CelebA (''is male'', ''is wearing heavy makeup'', and ''is
    wearing lipstick''). We consider pairs of examples {(x 1, y 1), (x 2, y 2)} (where
    one example is male and the other female) and produce random convex combinations
    of the attributesỹ mix = αy 1 + (1 − α)y 2 and decode their ing mixes Mix sup
    (f (x 1), f (x 2),ỹ mix ). This can be seen in FIG2.: Interpolations produced
    by the class mixer function for the set of binary attributes {male, heavy makeup,
    lipstick}. For each image, the left-most face is x 1 and the right-most face x
    2, with faces in between consisting of mixes Mix sup (f (x 1), f (x 2),ỹ mix )
    of a particular attribute mixỹ mix, shown below each column (where red denotes
    ''off'' and green denotes ''on'').We can see that for the most part, the class
    mixer function has been able to produce decent mixes between the two faces consistent
    with the desired attributes. There are some issues -namely, the model does not
    seem to disentangle the lipstick and makeup attributes well -but this may be due
    to the strong correlation between lipstick and makeup (lipstick is makeup!), or
    be in part due to the classification performance of the auxiliary classifier part
    of the discriminator (while its accuracy on both training and validation was as
    high as 95%, there may still be room for improvement). We also achieved better
    by simply having the embedding function produce a mask m ∈ rather than {0, 1},
    most likely because such a formulation allows a greater degree of flexibility
    when it comes to mixing. Indeed, one priority is to conduct further hyperparameter
    tuning in order to improve these . For a visualisation of the Bernoulli parameters
    output by the embedding function, see Section 5.3 in the appendix. In this paper,
    we proposed the adversarial mixup resynthesiser and showed that it can be used
    to produce realistic-looking combinations of examples by performing mixing in
    the bottleneck of an autoencoder. We proposed several mixing functions, including
    one based on sampling from a uniform distribution and the other a Bernoulli distribution.
    Furthermore, we presented a semisupervised version of the Bernoulli variant in
    which one can leverage class labels to learn a mixing function which can determine
    what parts of the latent code should be mixed to produce an image consistent with
    a desired class label. While our technique can be used to leverage an autoencoder
    as a generative model, we conjecture that our technique may have positive effects
    on the latent representation and therefore downstream tasks, though this is yet
    to be substantiated. Future work will involve more comparisons to existing literature
    and experiments to determine the effects of mixing on the latent space itself
    and downstream tasks. We will provide a summary of our experimental setup here,
    though we also provide links to (and encourage viewers to look at) various parts
    of the code such as the networks used for the generator and discriminator and
    the optimiser hyperparameters. We use a residual network for both the generator
    and discriminator. The discriminator uses spectral normalisation BID15, with five
    discriminator updates being performed for each generator update. We use ADAM for
    our optimiser with α = 2e −4, β 1 = 0.0 and β 2 = 0.99. In order to examine the
    effect of the consistency loss, we explore a simple two-dimensional spiral dataset,
    where points along the spiral are deemed to be part of the data distribution and
    points outside it are not. With the mixup loss enabled and λ = 10, we try values
    of β ∈ {0, 0.1, 10, 100}. After 100 epochs of training, we produce decoded random
    mixes and plot them over the data distribution, which are shown as orange points
    (overlaid on top of real samples, shown in blue). This is shown in FIG3.As we
    can see, the lower β is, the more likely interpolated points will lie within the
    data manifold (i.e. the spiral). This is because the consistency loss competes
    with the discriminator loss -as β is decreased, there is a relatively greater
    incentive for the autoencoder to try and fool the discriminator with interpolations,
    forcing it to decode interpolated points such that they lie in the spiral. Ideally
    however we would want a bit of both: we want high consistency so that interpolations
    in hidden states are semantically meaningful (and do not decode into some other
    random data point), while also having those decoded interpolations look realistic.
    Interpolations are defined as DISPLAYFORM0 We also compare our formulation to
    ACAI BID2, which does not explicitly have a consistency loss term. Instead, the
    discriminator tries to predict what the mixing coefficient α is, and the autoencoder
    tries to fool it into thinking interpolations have a coefficient of 0. In FIG5
    we compare this to our formulation in which β = 0. This is shown in FIG5 (right
    figure). It appears that ACAI also prefers to place points in the spiral, although
    not as strongly as AMR with β = 0 (though this may be because ACAI needs to trained
    for longer -ACAI and AMR were trained for the same number of epochs). In FIG5
    we can see that over the course of training the consistency losses for both ACAI
    and AMR gradually rise, indicating both models'' preference for moving interpolated
    points closer to the data manifold. Note that here we are only observing the consistency
    loss during training, and it is not used in the generator''s loss. Lastly, in
    Figure 9 we show some side-by-side comparisons of our model interpolating between
    faces when β = 50 and β = 0. We can see that when β = 0 interpolations between
    faces are not as smooth in terms of colour and lighting. This somewhat slight
    discontinuity in the interpolation may be explained by the decoder pushing these
    interpolated points closer to the data manifold, since there is no consistency
    loss enforced.(a) Left: AMR with λ = 10, β = 0; right: ACAI with λ = 10 (β = 0
    since ACAI does not enforce a consistency loss). AMR was trained for 200 epochs
    and ACAI for 300 epochs, since ACAI takes longer to converge. DISPLAYFORM1 and
    α ∼ U for randomly sampled {x 1, x 2}) over the course of training. DISPLAYFORM2
    Figure 9: Interpolations using AMR {λ = 50, β = 50} and {λ = 50, β = 0}. To recap,
    the class mixer in the supervised formulation internally maps from a labelỹ mix
    to Bernoulli parameters p ∈ K, from which a Bernoulli mask m ∼ Bernoulli(p) is
    sampled. The ing Bernoulli parameters p are shown in Figure 10, where each row
    denotes some combination of attributes y ∈ {000, 001, 010, . . .} and the columns
    denote the index of p (spread out across four images, such that the first image
    denotes p 1:128, second image p 128:256, etc.). We can see that each attribute
    combination spells out a binary combination of feature maps, which allows one
    to easily glean which feature maps contribute to which attributes. Figure 10:
    Visualisation of Bernoulli parameters p internally produced by the class mixer
    function. Rows denote attribute combinations y and columns denote the index of
    p. In this section we show additional samples of the AMR model (using mixup and
    Bernoulli mixup variants) on Zappos and CelebA datasets. We compare AMR against
    linear interpolation in pixel space (pixel), adversarial reconstruction autoencoder
    (ARAE), and adversarialy contrained autoencoder interpolation (ACAI). As can be
    observed in the following images, the interpolations of pixel and ARAE are less
    realistic and suffer from more artifacts. AMR and ACAI produce more realisticlooking
    , while AMR generates a smoother transition between the two samples.• Figure'
  sentences:
  - An improved Fast Weight network which shows better results on a general toy task.
  - We introduce NetScore, new metric designed to provide a quantitative assessment
    of the balance between accuracy, computational complexity, and network architecture
    complexity of a deep neural network.
  - We leverage deterministic autoencoders as generative models by proposing mixing
    functions which combine hidden states from pairs of images. These mixes are made
    to look realistic through an adversarial framework.
- source_sentence: 'Generative adversarial nets (GANs) are widely used to learn the
    data sampling process and their performance may heavily depend on the loss functions,
    given a limited computational budget. This study revisits MMD-GAN that uses the
    maximum mean discrepancy (MMD) as the loss function for GAN and makes two contributions.
    First, we argue that the existing MMD loss function may discourage the learning
    of fine details in data as it attempts to contract the discriminator outputs of
    real data. To address this issue, we propose a repulsive loss function to actively
    learn the difference among the real data by simply rearranging the terms in MMD.
    Second, inspired by the hinge loss, we propose a bounded Gaussian kernel to stabilize
    the training of MMD-GAN with the repulsive loss function. The proposed methods
    are applied to the unsupervised image generation tasks on CIFAR-10, STL-10, CelebA,
    and LSUN bedroom datasets. Results show that the repulsive loss function significantly
    improves over the MMD loss at no additional computational cost and outperforms
    other representative loss functions. The proposed methods achieve an FID score
    of 16.21 on the CIFAR-10 dataset using a single DCGAN network and spectral normalization.
    Generative adversarial nets (GANs) BID7 ) are a branch of generative models that
    learns to mimic the real data generating process. GANs have been intensively studied
    in recent years, with a variety of successful applications (; Li et al. (2017b);;;
    BID13 ). The idea of GANs is to jointly train a generator network that attempts
    to produce artificial samples, and a discriminator network or critic that distinguishes
    the generated samples from the real ones. Compared to maximum likelihood based
    methods, GANs tend to produce samples with sharper and more vivid details but
    require more efforts to train. Recent studies on improving GAN training have mainly
    focused on designing loss functions, network architectures and training procedures.
    The loss function, or simply loss, defines quantitatively the difference of discriminator
    outputs between real and generated samples. The gradients of loss functions are
    used to train the generator and discriminator. This study focuses on a loss function
    called maximum mean discrepancy (MMD), which is well known as the distance metric
    between two probability distributions and widely applied in kernel two-sample
    test BID8 ). Theoretically, MMD reaches its global minimum zero if and only if
    the two distributions are equal. Thus, MMD has been applied to compare the generated
    samples to real ones directly (; BID5) and extended as the loss function to the
    GAN framework recently (; Li et al. (2017a); ).In this paper, we interpret the
    optimization of MMD loss by the discriminator as a combination of attraction and
    repulsion processes, similar to that of linear discriminant analysis. We argue
    that the existing MMD loss may discourage the learning of fine details in data,
    as the discriminator attempts to minimize the within-group variance of its outputs
    for the real data. To address this issue, we propose a repulsive loss for the
    discriminator that explicitly explores the differences among real data. The proposed
    loss achieved significant improvements over the MMD loss on image generation tasks
    of four benchmark datasets, without incurring any additional computational cost.
    Furthermore, a bounded Gaussian kernel is proposed to stabilize the training of
    discriminator. As such, using a single kernel in MMD-GAN is sufficient, in contrast
    to a linear combination of kernels used in Li et al. (2017a) and. By using a single
    kernel, the computational cost of the MMD loss can potentially be reduced in a
    variety of applications. The paper is organized as follows. Section 2 reviews
    the GANs trained using the MMD loss (MMD-GAN). We propose the repulsive loss for
    discriminator in Section 3, introduce two practical techniques to stabilize the
    training process in Section 4, and present the of extensive experiments in Section
    5. In the last section, we discuss the connections between our model and existing
    work. In this section, we introduce the GAN model and MMD loss. Consider a random
    variable X ∈ X with an empirical data distribution P X to be learned. A typical
    GAN model consists of two neural networks: a generator G and a discriminator D.
    The generator G maps a latent code z with a fixed distribution P Z (e.g., Gaussian)
    to the data space X: y = G(z) ∈ X, where y represents the generated samples with
    distribution P G. The discriminator D evaluates the scores D(a) ∈ R d of a real
    or generated sample a. This study focuses on image generation tasks using convolutional
    neural networks (CNN) for both G and D.Several loss functions have been proposed
    to quantify the difference of the scores between real and generated samples: {D(x)}
    and {D(y)}, including the minimax loss and non-saturating loss BID7 ), hinge loss
    , Wasserstein loss; BID10 ) and maximum mean discrepancy (MMD) (Li et al. (2017a);
    ) (see Appendix B.1 for more details). Among them, MMD uses kernel embedding φ(a)
    = k(·, a) associated with a characteristic kernel k such that φ is infinite-dimensional
    and φ(a), φ(b) H = k(a, b). The squared MMD distance between two distributions
    P and Q is DISPLAYFORM0 The kernel k(a, b) measures the similarity between two
    samples a and b. BID8 proved that, using a characteristic kernel k, M,P G ) reaches
    its minimum if and only if P X = P G (Li et al. (2017a) ). Thus, the objective
    functions for G and D could be (Li et al. (2017a); ): min DISPLAYFORM1 ) MMD-GAN
    has been shown to be more effective than the model that directly uses MMD as the
    loss function for the generator G (Li et al. (2017a)). showed that MMD and Wasserstein
    metric are weaker objective functions for GAN than the Jensen-Shannon (JS) divergence
    (related to minimax loss) and total variation (TV) distance (related to hinge
    loss). The reason is that convergence of P G to P X in JS-divergence and TV distance
    also implies convergence in MMD and Wasserstein metric. Weak metrics are desirable
    as they provide more information on adjusting the model to fit the data distribution
    . proved that the GAN trained using the minimax loss and gradient updates on model
    parameters is locally exponentially stable near equilibrium, while the GAN using
    Wasserstein loss is not. In Appendix A, we demonstrate that the MMD-GAN trained
    by gradient descent is locally exponentially stable near equilibrium. In this
    section, we interpret the training of MMD-GAN (using L First, consider a linear
    discriminant analysis (LDA) model as the discriminator. The task is to find a
    projection w to maximize the between-group variance w T µ x − w T µ y and minimize
    the withingroup variance w T (Σ x + Σ y)w, where µ and Σ are group mean and covariance.
    In MMD-GAN, the neural-network discriminator works in a similar way as LDA. By
    minimizing L att D, the discriminator D tackles two tasks: DISPLAYFORM0, causes
    the two groups {D(x)} and {D(y)} to repel each other (see FIG1, or maximize betweengroup
    variance; and 2) D increases DISPLAYFORM1 e. contracts {D(x)} and {D(y)} within
    each group (see FIG1, or minimize the within-group variance. We refer to loss
    functions that contract real data scores as attractive losses. We argue that the
    attractive loss L att D (Eq. 3) has two issues that may slow down the GAN training:1.
    The discriminator D may focus more on the similarities among real samples (in
    order to contract {D(x)}) than the fine details that separate them. Initially,
    G produces low-quality samples and it may be adequate for D to learn the common
    features of {x} in order to distinguish between {x} and {y}. Only when {D(y)}
    is sufficiently close to {D(x)} will D learn the fine details of {x} to be able
    to separate {D(x)} from {D(y)}. Consequently, D may leave out some fine details
    in real samples, thus G has no access to them during training.2. As shown in FIG1,
    the gradients on D(y) from the attraction (blue arrows) and repulsion (orange
    arrows) terms in L att D (and thus L mmd G) may have opposite directions during
    training. Their summation may be small in magnitude even when D(y) is far away
    from D(x), which may cause G to stagnate locally. Therefore, we propose a repulsive
    loss for D to encourage repulsion of the real data scores {D(x)}: DISPLAYFORM2
    The generator G uses the same MMD loss L mmd G as before (see Eq. 2). Thus, the
    adversary lies in the fact that D contracts {D(y)} via maximizing FIG1 ) while
    G expands {D(y)} (see FIG1). Additionally, D also learns to separate the real
    data by minimizing DISPLAYFORM3 DISPLAYFORM4, which actively explores the fine
    details in real samples and may in more meaningful gradients for G. Note that
    in Eq. 4, D does not explicitly push the average score of {D(y)} away from that
    of {D(x)} because it may have no effect on the pair-wise sample distances. But
    G aims to match the average scores of both groups. Thus, we believe, compared
    to the model using DISPLAYFORM5 and L rep D is less likely to yield opposite gradients
    when {D(y)} and {D(x)} are distinct (see FIG1). In Appendix A, we demonstrate
    that GAN trained using gradient descent and the repulsive MMD loss (L At last,
    we identify a general form of loss function for the discriminator D: DISPLAYFORM6
    In this section, we propose two approaches to stabilize the training of MMD-GAN:
    1) a bounded kernel to avoid the saturation issue caused by an over-confident
    discriminator; and 2) a generalized power iteration method to estimate the spectral
    norm of a convolutional kernel, which was used in spectral normalization on the
    discriminator in all experiments in this study unless specified otherwise. For
    MMD-GAN, the following two kernels have been used:• Gaussian radial basis function
    (RBF), or Gaussian kernel (Li et al. (2017a) DISPLAYFORM0 2 ) where σ > 0 is the
    kernel scale or bandwidth.• Rational quadratic kernel DISPLAYFORM1, where the
    kernel scale α > 0 corresponds to a mixture of Gaussian kernels with a Gamma(α,
    1) prior on the inverse kernel scales σ −1.It is interesting that both studies
    used a linear combination of kernels with five different kernel scales, i.e.,
    DISPLAYFORM2, where σ i ∈ {1, 2, 4, 8, 16}, α i ∈ {0.2, 0.5, 1, 2, 5} (see FIG0
    and 2c for illustration). We suspect the reason is that a single kernel k(a, b)
    is saturated when the distance a − b is either too large or too small compared
    to the kernel scale (see FIG0 and 2d), which may cause diminishing gradients during
    training. Both Li et al. (2017a) and applied penalties on the discriminator parameters
    but not to the MMD loss itself. Thus the saturation issue may still exist. Using
    a linear combination of kernels with different kernel scales may alleviate this
    issue but not eradicate it. Inspired by the hinge loss (see Appendix B.1), we
    propose a bounded RBF (RBF-B) kernel for the discriminator. The idea is to prevent
    D from pushing {D(x)} too far away from {D(y)} and causing saturation. For L att
    D in Eq. 3, the RBF-B kernel is: DISPLAYFORM3 For L rep D in Eq. 4, the RBF-B
    kernel is: DISPLAYFORM4 where b l and b u are the lower and upper bounds. As such,
    a single kernel is sufficient and we set σ = 1, b l = 0.25 and b u = 4 in all
    experiments for simplicity and leave their tuning for future work. It should be
    noted that, like the case of hinge loss, the RBF-B kernel is used only for the
    discriminator to prevent it from being over-confident. The generator is always
    trained using the original RBF kernel, thus we retain the interpretation of MMD
    loss L mmd G as a metric. RBF-B kernel is among many methods to address the saturation
    issue and stabilize MMD-GAN training. We found random sampling kernel scale, instance
    noise (Sønderby et al. ) and label smoothing may also improve the model performance
    and stability. However, the computational cost of RBF-B kernel is relatively low.
    Without any Lipschitz constraints, the discriminator D may simply increase the
    magnitude of its outputs to minimize the discriminator loss, causing unstable
    training 3. Spectral normalization divides the weight matrix of each layer by
    its spectral norm, which imposes an upper bound on the magnitudes of outputs and
    gradients at each layer of D . However, to estimate the spectral norm of a convolution
    kernel, reshaped the kernel into a matrix. We propose a generalized power iteration
    method to directly estimate the spectral norm of a convolution kernel (see Appendix
    C for details) and applied spectral normalization to the discriminator in all
    experiments. In Appendix D.1, we explore using gradient penalty to impose the
    Lipschitz constraint BID10;; ) for the proposed repulsive loss. In this section,
    we empirically evaluate the proposed 1) repulsive loss L (Li et al. (2017a) )
    and rational quadratic kernel (MMD-rq) ), as well as non-saturating loss BID7
    ) and hinge loss . To show the efficacy of RBF-B kernel, we applied it to both
    L ). DISPLAYFORM0 Dataset: The loss functions were evaluated on four datasets:
    1) CIFAR-10 (50K images, 32 × 32 pixels) ; 2) STL-10 (100K images, 48 × 48 pixels)
    BID3 ); 3) CelebA (about 203K images, 64 × 64 pixels) (Liu et al. FORMULA8); and
    4) LSUN bedrooms (around 3 million images, 64×64 pixels) (Yu et al. FORMULA8).
    The images were scaled to range [−1, 1] to avoid numeric issues. FORMULA8 ) was
    used in the generator, and spectral normalization with the generalized power iteration
    (see Appendix C) in the discriminator. For MMD related losses, the dimension of
    discriminator output layer was set to 16; for non-saturating loss and hinge loss,
    it was 1. In Appendix D.2, we investigate the impact of discriminator output dimension
    on the performance of repulsive loss. ) and thus omitted.3 On LSUN-bedroom, MMD-rbf
    and MMD-rq did not achieve reasonable and thus are omitted. Hyper-parameters:
    We used Adam optimizer (Kingma & Ba FORMULA8) with momentum parameters β 1 = 0.5,
    β 2 = 0.999; two-timescale update rule (TTUR) BID12 ) with two learning rates
    (ρ D, ρ G) chosen from {1e-4, 2e-4, 5e-4, 1e-3} (16 combinations in total); and
    batch size 64. Fine-tuning on learning rates may improve the model performance,
    but constant learning rates were used for simplicity. All models were trained
    for 100K iterations on CIFAR-10, STL-10, CelebA and LSUN bedroom datasets, with
    n dis = 1, i.e., one discriminator update per generator update 4. For MMD-rbf,
    the kernel scales σ i ∈ {1, √ 2, 2, 2 √ 2, 4} were used due to a better performance
    than the original values used in Li et al. (2017a). For MMD-rq, α i ∈ {0.2, 0.5,
    1, 2, 5}. For MMD-rbf-b, MMD-rep, MMD-rep-b, a single Gaussian kernel with σ =
    1 was used. Evaluation metrics: Inception score (IS) , Fréchet Inception distance
    (FID) BID12 ) and multi-scale structural similarity (MS-SSIM) were used for quantitative
    evaluation. Both IS and FID are calculated using a pre-trained Inception model
    . Higher IS and lower FID scores indicate better image quality. MS-SSIM calculates
    the pair-wise image similarity and is used to detect mode collapses among images
    of the same class . Lower MS-SSIM values indicate perceptually more diverse images.
    For each model, 50K randomly generated samples and 50K real samples were used
    to calculate IS, FID and MS-SSIM. TAB0 shows the Inception score, FID and MS-SSIM
    of applying different loss functions on the benchmark datasets with the optimal
    learning rate combinations tested experimentally. Note that the same training
    setup (i.e., DCGAN + BN + SN + TTUR) was applied for each loss function. We observed
    that: 1) MMD-rep and MMD-rep-b performed significantly better than MMD-rbf and
    MMD-rbf-b respectively, showing the proposed repulsive loss L rep D (Eq. 4) greatly
    improved over the attractive loss L att D (Eq. 3); 2) Using a single kernel, MMD-rbf-b
    performed better than MMD-rbf and MMD-rq which used a linear combination of five
    kernels, indicating that the kernel saturation may be an issue that slows down
    MMD-GAN training; 3) MMD-rep-b performed comparable or better than MMD-rep on
    benchmark datasets where we found the RBF-B kernel managed to stabilize MMD-GAN
    training using repulsive loss. 4) MMD-rep and MMD-rep-b performed significantly
    better than the non-saturating and hinge losses, showing the efficacy of the proposed
    repulsive loss. Additionally, we trained MMD-GAN using the general loss L D,λ
    (Eq. 5) for discriminator and L mmd G (Eq. 2) for generator on the CIFAR-10 dataset.
    Each color bar represents the FID score using a learning rate combination (ρ D,
    ρ G), in the order of (1e-4, 1e-4), (1e-4, 2e-4),...,(1e-3, 1e-3). The discriminator
    was trained using L D,λ (Eq. 5) with λ ∈ {-1, -0.5, 0, 0.5, 1, 2}, and generator
    using L mmd G (Eq. 2). We use the FID> 30 to indicate that the model diverged
    or produced poor .of MMD-GAN with RBF and RBF-B kernel 5. Note that when λ = −1,
    the models are essentially MMD-rbf (with a single Gaussian kernel) and MMD-rbf-b
    when RBF and RBF-B kernel are used respectively. We observed that: 1) the model
    performed well using repulsive loss (i.e., λ ≥ 0), with λ = 0.5, 1 slightly better
    than λ = −0.5, 0, 2; 2) the MMD-rbf model can be significantly improved by simply
    increasing λ from −1 to −0.5, which reduces the attraction of discriminator on
    real sample scores; 3) larger λ may lead to more diverged models, possibly because
    the discriminator focuses more on expanding the real sample scores over adversarial
    learning; note when λ 1, the model would simply learn to expand all real sample
    scores and pull the generated sample scores to real samples'', which is a divergent
    process; 4) the RBF-B kernel managed to stabilize MMD-rep for most diverged cases
    but may occasionally cause the FID score to rise up. The proposed methods were
    further evaluated in Appendix A, C and D. In Appendix A.2, we used a simulation
    study to show the local stability of MMD-rep trained by gradient descent, while
    its global stability is not guaranteed as bad initialization may lead to trivial
    solutions. The problem may be alleviated by adjusting the learning rate for generator.
    In Appendix C.3, we showed the proposed generalized power iteration (Section 4.2)
    imposes a stronger Lipschitz constraint than the method in , and benefited MMD-GAN
    training using the repulsive loss. Moreover, the RBF-B kernel managed to stabilize
    the MMD-GAN training for various configurations of the spectral normalization
    method. In Appendix D.1, we showed the gradient penalty can also be used with
    the repulsive loss. In Appendix D.2, we showed that it was better to use more
    than one neuron at the discriminator output layer for the repulsive loss. The
    discriminator outputs may be interpreted as a learned representation of the input
    samples. FIG8 visualizes the discriminator outputs learned by the MMD-rbf and
    proposed MMD-rep methods on CIFAR-10 dataset using t-SNE (van der Maaten FORMULA4).
    MMD-rbf ignored the class structure in data (see FIG8) while MMD-rep learned to
    concentrate the data from the same class and separate different classes to some
    extent FIG8. This is because the discriminator D has to actively learn the data
    structure in order to expands the real sample scores {D(x)}. Thus, we speculate
    that techniques reinforcing the learning of cluster structures in data may further
    improve the training of MMD-GAN.In addition, the performance gain of proposed
    repulsive loss (Eq. 4) over the attractive loss (Eq. 3) comes at no additional
    computational cost. In fact, by using a single kernel rather than a linear combination
    of kernels, MMD-rep and MMD-rep-b are simpler than MMD-rbf and MMD-rq. Besides,
    given a typically small batch size and a small number of discriminator output
    neurons (64 and 16 in our experiments), the cost of MMD over the non-saturating
    and hinge loss is marginal compared to the convolution operations. In Appendix
    D.3, we provide some random samples generated by the methods in our study. This
    study extends the previous work on MMD-GAN (Li et al. (2017a) ) with two contributions.
    First, we interpreted the optimization of MMD loss as a combination of attraction
    and repulsion processes, and proposed a repulsive loss for the discriminator that
    actively learns the difference among real data. Second, we proposed a bounded
    Gaussian RBF (RBF-B) kernel to address the saturation issue. Empirically, we observed
    that the repulsive loss may in unstable training, due to factors including initialization
    (Appendix A.2), learning rate (FIG7 and Lipschitz constraints on the discriminator
    (Appendix C.3). The RBF-B kernel managed to stabilize the MMD-GAN training in
    many cases. Tuning the hyper-parameters in RBF-B kernel or using other regularization
    methods may further improve our . The theoretical advantages of MMD-GAN require
    the discriminator to be injective. The proposed repulsive loss (Eq. 4) attempts
    to realize this by explicitly maximizing the pair-wise distances among the real
    samples. Li et al. (2017a) achieved the injection property by using the discriminator
    as the encoder and an auxiliary network as the decoder to reconstruct the real
    and generated samples, which is more computationally extensive than our proposed
    approach. On the other hand,; imposed a Lipschitz constraint on the discriminator
    in MMD-GAN via gradient penalty, which may not necessarily promote an injective
    discriminator. The idea of repulsion on real sample scores is in line with existing
    studies. It has been widely accepted that the quality of generated samples can
    be significantly improved by integrating labels (; ;) or even pseudo-labels generated
    by k-means method BID9 ) in the training of discriminator. The reason may be that
    the labels help concentrate the data from the same class and separate those from
    different classes. Using a pre-trained classifier may also help produce vivid
    image samples BID14 ) as the learned representations of the real samples in the
    hidden layers of the classifier tend to be well separated/organized and may produce
    more meaningful gradients to the generator. At last, we note that the proposed
    repulsive loss is orthogonal to the GAN studies on designing network structures
    and training procedures, and thus may be combined with a variety of novel techniques.
    For example, the ResNet architecture BID11 ) has been reported to outperform the
    plain DCGAN used in our experiments on image generation tasks (; BID10) and self-attention
    module may further improve the . On the other hand, proposed to progressively
    grows the size of both discriminator and generator and achieved the state-of-the-art
    performance on unsupervised training of GANs on the CIFAR-10 dataset. Future work
    may explore these directions. This section demonstrates that, under mild assumptions,
    MMD-GAN trained by gradient descent is locally exponentially stable at equilibrium.
    It is organized as follows. The main assumption and proposition are presented
    in Section A.1, followed by simulation study in Section A.2 and proof in Section
    A.3. We discuss the indications of assumptions on the discriminator of GAN in
    Section A.4. We consider GAN trained using the MMD loss L DISPLAYFORM0 where Thus
    in contrast to Assumption 1, we assume Assumption 2. For GANs using MMD loss in
    Eq. S1, and random initialization on parameters, at equilibrium, DISPLAYFORM1
    DISPLAYFORM2 is not constant almost everywhere. We use a simulation study in Section
    A.2 to show that D θ * D (x) = 0 does not hold in general for MMD loss. Based
    on Assumption 2, we propose the following proposition and prove it in Appendix
    A.3: Proposition 1. If there exists θ * G ∈ Θ G such that P θ * G = P X, then
    GANs with MMD loss in Eq. S1 has equilibria (θ * G, θ D) for any θ D ∈ Θ D. Moreover,
    the model trained using gradient descent methods is locally exponentially stable
    at (θ * DISPLAYFORM3 There may exist non-realizable cases where the mapping between
    P Z and P X cannot be represented by any generator G θ G with θ G ∈ Θ G . In Section
    A.2, we use a simulation study to show that both the attractive MMD loss L att
    D (Eq. S1b) and the proposed repulsive loss L rep D (Eq. S1c) may be locally stable
    and leave the proof for future work. In this section, we reused the example from
    to show that GAN trained using the MMD loss in Eq. S1 is locally stable. Consider
    a two-parameter MMD-GAN with uniform latent distribution P Z over [−1, 1], generator
    G(z) = w 1 z, discriminator D(x) = w 2 x 2, and Gaussian kernel k (a) the data
    distribution P X is the same as P Z, i.e., uniform over [−1, 1], thus P X is realizable;
    DISPLAYFORM0 Figure S1: Streamline plots of MMD-GAN using the MMD-rbf and the
    MMD-rep model on distributions: P Z = U(−1, 1), P X = U(−1, 1) or P X = N. In
    (a) and (b), the equilibria satisfying P G = P X lie on the line w 1 = 1. In (c),
    the equilibrium lies around point (1.55, 0.74); in (d), it is around (1.55, 0.32).(b)
    P X is standard Gaussian, thus non-realizable for any w 1 ∈ R. FIG1 shows that
    MMD-GAN are locally stable in both cases and D θ * D (x) = 0 does not hold in
    general for MMD loss. However, MMD-rep may not be globally stable for the tested
    cases: initialization of (w 1, w 2) in some regions may lead to the trivial solution
    w 2 = 0 (see FIG1 and S1d). We note that by decreasing the learning rate for G,
    the area of such regions decreased. At last, it is interesting to note that both
    MMD-rbf and MMD-rep had the same nontrivial solution w 1 ≈ 1.55 for generator
    in the non-realizable cases (see FIG1 and S1d). This section divides the proof
    for Proposition 1 into two parts. First, we show that GAN with the MMD loss in
    Eq. S1 has equilibria for any parameter configuration of discriminator D; second,
    we prove the model is locally exponentially stable. For convenience, we consider
    the general form of discriminator loss in Eq. 5: DISPLAYFORM0 which has L att
    D and L rep D as the special cases when λ equals −1 and 1 respectively. Consider
    real data X r ∼ P X, latent variable Z ∼ P Z and generated variable Y g = G θ
    G (Z). Let x r, z, y g be their samples. Denote ∇. DISPLAYFORM1 where L D and
    L G are the losses for D and G respectively. Assume an isotropic stationary kernel
    k(a, b) = k I (a − b) BID6 ) is used in MMD. We first show:Proposition 1 (Part
    1). If there exists θ * G ∈ Θ G such that P θ * G = P X, the GAN with the MMD
    loss in Eq. S1a and Eq. S2 has equilibria (θ * DISPLAYFORM2 where k is the kernel
    of MMD. The gradients of MMD loss are DISPLAYFORM3 ∼ P G, an unbiased estimator
    of the squared MMD is BID8) We proceed to prove the model stability. First, following
    Theorem 5 in BID8 and Theorem 4 in Li et al. (2017a), it is straightforward to
    see: FORMULA13 ). Consider a non-linear system of parameters (θ, γ): θ = h 1 (θ,
    γ),γ = h 2 (θ, γ) with an equilibrium point at. Let there exist such that ∀γ ∈
    DISPLAYFORM4 DISPLAYFORM5 DISPLAYFORM6 is a Hurwitz matrix, the non-linear system
    is exponentially stable. Proposition 1 (Part 2). At equilibrium P θ * G = P X,
    the GAN trained using MMD loss and gradient descent methods is locally exponentially
    stable at (θ DISPLAYFORM0 ∂b∂c . Based on Eq. S3, we have DISPLAYFORM1 where ⊗
    is the kronecker product. At equilibrium, consider a sequence of N samples DISPLAYFORM2
    DISPLAYFORM3 Given Lemma A.1 and fact that J GG is the Hessian matrix of M DISPLAYFORM4
    is locally constant along some directions in the parameter space of G. As a ,
    null(J GG) ⊆ null(J DG) because varying θ * G along these directions has no effect
    on D. Following Lemma C.3 of , we consider eigenvalue decomposition DISPLAYFORM5.
    Thus, the projections γ G = T G θ G are orthogonal to null(J GG). Then, the Jacobian
    corresponding to the projected system has the form DISPLAYFORM6, where J GG is
    negative definite. Moreover, on all directions exclude those described by J GG,
    the system is surrounded by a neighborhood of equilibia at least locally. According
    to Lemma A.2, the system is exponentially stable. This section shows that constant
    discriminator output DISPLAYFORM0 may have no discrimination power. First, we
    make the following assumptions:Assumption 3. 1. D is a multilayer perceptron where
    each layer l can be factorized into an affine transform and an element-wise activation
    function f l. 2. Each activation function f l ∈ C 0; furthermore, f l has a finite
    number of discontinuities and f l ∈ C 06. 3. Input data to D is continuous and
    its support S is compact in R d with non-zero measure in each dimension and d
    > 1 7.Based on Assumption 3, we have the following proposition:Proposition 2.
    If ∀x ∈ S, D(x) = c, where c is constant, then there always exists distortion
    δx such that x + δx ∈ S and D(x + δx) = c. are model weights and biases, f is
    an activation function satisfying Assumption 3. For x ∈ S, since D(x) = c, we
    have h(x) ∈ null(W 2). Furthermore: DISPLAYFORM0 has unique solution for any k
    ∈ R as long as k · h(x) is within the output range of f. DISPLAYFORM1 and n is
    the nullity of W 2. Let the projected support beŜ. Thus, DISPLAYFORM2 T with z
    c = 0.Consider the Jacobian: DISPLAYFORM3 where DISPLAYFORM4 is the input to activation,
    or pre-activations. SinceŜ is continuous and compact, it has infinite number of
    boundary points {x b} for d > 1. Consider one boundary pointx b and its normal
    line δx b. Let > 0 be a small scalar such thatx b − δx b ∈ S andx b + δx b ∈Ŝ.•
    For linear activation, ∇Σ = I and J is constant. Then z c remains 0 DISPLAYFORM5
    there exists z such that h(x + δx) ∈ null(W 2).• For nonlinear activations, assume
    f has N discontinuities. Since U x T 0 T T + b 1 = c has unique solution for any
    vector c, the boundary points {x b} cannot yield pre-activations {a b} that all
    lie on the discontinuities in any of the d h directions. Though we might need
    to sample d N +1 h points in the worst case to find an exception, there are infinite
    number of exceptions. Letx b be a sample where {a b} does not lie on the discontinuities
    in any direction. Because f is continuous, z c remains 0 forx b + δx b, i.e.,
    there exists z such that h(x + δx) ∈ null(W 2).In , we can always find δx such
    that x + δx / ∈ S and D(x + δx) = c. cannot discriminate against fake samples
    with distortions to the original data. In contrast, Assumption 2 and Lemma A.1
    guarantee that, at equilibrium, the discriminator trained using MMD loss function
    is effective against such fake samples given a large number of i.i.d. test samples
    BID8 ). Several loss functions have been proposed to quantify the difference between
    real and generated sample scores, including: (assume linear activation is used
    at the last layer of D)• The Minimax loss BID7 ): Softplus(D(G(z) ))] and L G
    = −L D, which can be derived from the Jensen-Shannon (JS) divergence between P
    X and the model distribution P G. DISPLAYFORM0 • The non-saturating loss BID7
    ), which is a variant of the minimax loss with the same L D and DISPLAYFORM1 •
    The Hinge loss : DISPLAYFORM2, which is notably known for usage in support vector
    machines and is related to the total variation (TV) distance .• The Wasserstein
    loss; BID10 ), which is derived from the Wasserstein distance between P X and
    P G: DISPLAYFORM3, where D is subject to some Lipschitz constraint.• The maximum
    mean discrepancy (MMD) (Li et al. (2017a); ), as described in Section 2. For unsupervised
    image generation tasks on CIFAR-10 and STL-10 datasets, the DCGAN architecture
    from was used. For CelebA and LSUN bedroom datasets, we added more layers to the
    generator and discriminator accordingly. See TAB0 and S2 for details. TAB0: DCGAN
    models for image generation on CIFAR-10 (h = w = 4, H = W = 32) and STL-10 (h
    = w = 6, H = W = 48) datasets. For non-saturating loss and hinge loss, s = 1;
    for MMD-rand, MMD-rbf, MMD-rq, s = 16. For a weight matrix W, the spectral norm
    is defined as σ(W) = max v 2 ≤1 W v 2. The PIM is used to estimate σ(W) , which
    iterates between two steps: DISPLAYFORM0 The convolutional kernel W c is a tensor
    of shape h × w × c in × c out with h, w the receptive field size and c in, c out
    the number of input/output channels. To estimate σ(W c), reshaped it into a matrix
    W rs of shape (hwc in) × c out and estimated σ(W rs).We propose a simple method
    to calculate W c directly based on the fact that convolution operation is linear.
    For any linear map T: R m → R n, there exists matrix W L ∈ R n×m such that y =
    T (x) can be represented as y = W L x. Thus, we may simply substitute W L = ∂y
    ∂x in the PIM method to estimate the spectral norm of any linear operation. In
    the case of convolution operation *, there exist doubly block circulant matrix
    DISPLAYFORM1 T u which is essentially the transpose convolution of W c on u BID4
    ). Thus, similar to PIM, PICO iterates between the following two steps: DISPLAYFORM2
    2. Do transpose convolution of W c on u to getv; update v =v/ v 2.Similar approaches
    have been proposed in and from different angles, which we were not aware during
    this study. In addition, proposes to compute the exact singular values of convolution
    kernels using FFT and SVD. In spectral normalization, only the first singular
    value is concerned, making the power iteration methods PIM and PICO more efficient
    than FFT and thus preferred in our study. However, we believe the exact method
    FFT+SVD may eventually inspire more rigorous regularization methods for GAN.The
    proposed PICO method estimates the real spectral norm of a convolution kernel
    at each layer, thus enforces an upper bound on the Lipschitz constant of the discriminator
    D. Denote the upper bound as LIP PICO. In this study, Leaky ReLU (LReLU) was used
    at each layer of D, thus LIP PICO ≈ 1 . In practice, however, PICO would often
    cause the norm of the signal passing through D to decrease to zero, because at
    each layer,• the signal hardly coincides with the first singular-vector of the
    convolution kernel; and• the activation function LReLU often reduces the norm
    of the signal. Consequently, the discriminator outputs tend to be similar for
    all the inputs. To compensate the loss of norm at each layer, the signal is multiplied
    by a constant C after each spectral normalization. This essentially enlarges LIP
    PICO by C K where K is the number of layers in the DCGAN discriminator. For all
    experiments in Section 5, we fixed C = 1 0.55 ≈ 1.82 as all loss functions performed
    relatively well empirically. In Appendix Section C.3, we tested the effects of
    coefficient C K on the performance of several loss functions. PIM also enforces
    an upper bound LIP PIM on the Lipschitz constant of the discriminator D. Consider
    a convolution kernel W c with receptive field size h × w and stride s. Let σ PICO
    and σ PIM be the spectral norm estimated by PICO and PIM respectively. We empirically
    In this section, we empirically evaluate the effects of coefficient C K on the
    performance of PICO and compare PICO against PIM using several loss functions.
    We used a similar setup as Section 5.1 with the following adjustments. Four loss
    functions were tested: hinge, MMD-rbf, MMD-rep and MMD-rep-b. Either PICO or PIM
    was used at each layer of the discriminator. For PICO, five coefficients C K were
    tested: 16, 32, 64, 128 and 256 (note this is the overall coefficient for K layers;
    K = 8 for CIFAR-10 and STL-10; K = 10 for CelebA and LSUN-bedroom; see Appendix
    B.2). FID was used to evaluate the performance of each combination of loss function
    and power iteration method, e.g., hinge + PICO with C K = 16.Results: For each
    combination of loss function and power iteration method, the distribution of FID
    scores over 16 learning rate combinations is shown in FIG0. We separated well-performed
    learning rate combinations from diverged or poorly-performed ones using a threshold
    τ as the diverged cases often had non-meaningful FID scores. The boxplot shows
    the distribution of FID scores for goodperformed cases while the number of diverged
    or poorly-performed cases was shown above each box if it is non-zero. 1) When
    PICO was used, the hinge, MMD-rbf and MMD-rep methods were sensitive to the choices
    of C K while MMD-rep-b was robust. For hinge and MMD-rbf, higher C K may in better
    FID scores and less diverged cases over 16 learning rate combinations. For MMD-rep,
    higher C K may cause more diverged cases; however, the best FID scores were often
    achieved with C K = 64 or 128.2) For CIFAR-10, STL-10 and CelebA datasets, PIM
    performed comparable to PICO with C K = 128 or 256 on four loss functions. For
    LSUN bedroom dataset, it is likely that the performance of PIM corresponded to
    that of PICO with C K > 256. This implies that PIM may in a relatively loose Lipschitz
    constraint on deep convolutional networks.3) MMD-rep-b performed generally better
    than hinge and MMD-rbf with tested power iteration methods and hyper-parameter
    configurations. Using PICO, MMD-rep also achieved generally better FID scores
    than hinge and MMD-rbf. This implies that, given a limited computational budget,
    the proposed repulsive loss may be a better choice than the hinge and MMD loss
    for the discriminator. TAB2 shows the best FID scores obtained by PICO and PIM
    where C K was fixed at 128 for hinge and MMD-rbf, and 64 for MMD-rep and MMD-rep-b.
    For hinge and MMD-rbf, PICO performed significantly better than PIM on the LSUN-bedroom
    dataset and comparably on the rest datasets. For MMD-rep and MMD-rep-b, PICO achieved
    consistently better FID scores than PIM.However, compared to PIM, PICO has a higher
    computational cost which roughly equals the additional cost incurred by increasing
    the batch size by two . This may be problematic when a small batch has to be used
    due to memory constraints, e.g., when handling high resolution images on a single
    GPU. Thus, we recommend using PICO when the computational cost is less of a concern.
    D SUPPLEMENTARY EXPERIMENTS D.1 LIPSCHITZ CONSTRAINT VIA GRADIENT PENALTY Gradient
    penalty has been widely used to impose the Lipschitz constraint on the discriminator
    arguably since Wasserstein GAN BID10 ). This section explores whether the proposed
    repulsive loss can be applied with gradient penalty. Several gradient penalty
    methods have been proposed for MMD-GAN. penalized the gradient norm of witness
    function y) ] w.r.t. the interpolated sample z = ux + (1 − u)y to one, where u
    ∼ U 9. More recently, proposed to impose the Lipschitz constraint on the mapping
    φ • D directly and derived the Scaled MMD (SMMD) as SM k (P, Q) = σ µ,k,λ M k
    (P, Q), where the scale σ µ,k,λ incorporates gradient and smooth penalties. Using
    the Gaussian kernel and measure µ = P X leads to the discriminator loss: DISPLAYFORM0
    DISPLAYFORM1 We apply the same formation of gradient penalty to the repulsive
    loss: DISPLAYFORM2 where the numerator L rep D − 1 ≤ 0 so that the discriminator
    will always attempt to minimize both L rep D and the Frobenius norm of gradients
    ∇D(x) w.r.t. real samples. Meanwhile, the generator is trained using the MMD loss
    L mmd G (Eq. 2).Experiment setup: The gradient-penalized repulsive loss L rep-gp
    D (Eq. S8, referred to as MMD-repgp) was evaluated on the CIFAR-10 dataset. We
    found λ = 10 in too restrictive 9 Empirically, we found this gradient penalty
    did not work with the repulsive loss. The reason may be the attractive loss L
    att D (Eq. 3) is symmetric in the sense that swapping P X and PG in the same loss;
    while the repulsive loss is asymmetric and naturally in varying gradient norms
    in data space. and used λ = 0.1 instead. Same as, the output dimension of discriminator
    was set to one. Since we entrusted the Lipschitz constraint to the gradient penalty,
    spectral normalization was not used. The rest experiment setup can be found in
    Section 5.1. TAB3 shows that the proposed repulsive loss can be used with gradient
    penalty to achieve reasonable on CIFAR-10 dataset. For comparison, we cited the
    Inception score and FID for Scaled MMD-GAN (SMMDGAN) and Scaled MMD-GAN with spectral
    normalization (SN-SMMDGAN) from. Note that SMMDGAN and SN-SMMDGAN used the same
    DCGAN architecture as MMD-rep-gp, but were trained for 150k generator updates
    and 750k discriminator updates, much more than that of MMD-rep-gp (100k for both
    G and D). Thus, the repulsive loss significantly improved over the attractive
    MMD loss for discriminator. In this section, we investigate the impact of the
    output dimension of discriminator on the performance of repulsive loss. Experiment
    setup: We used a similar setup as Section 5.1 with the following adjustments.
    The repulsive loss was tested on the CIFAR-10 dataset with a variety of discriminator
    output dimensions: d ∈ {1, 4, 16, 64, 256}. Spectral normalization was applied
    to discriminator with the proposed PICO method (see Appendix C) and the coefficients
    C K selected from {16, 32, 64, 128, 256}.Results: TAB4 shows that using more than
    one output neuron in the discriminator D significantly improved the performance
    of repulsive loss over the one-neuron case on CIFAR-10 dataset. The reason may
    be that using insufficient output neurons makes it harder for the discriminator
    to learn an injective and discriminative representation of the data (see FIG8).
    However, the performance gain diminished when more neurons were used, perhaps
    because it becomes easier for D to surpass the generator G and trap it around
    saddle solutions. The computation cost also slightly increased due to more output
    neurons. Generated samples on CelebA dataset are given in FIG7 and LSUN bedrooms
    in FIG8. Spectral normalization was applied to discriminator with two power iteration
    methods: PICO and PIM. For PICO, five coefficients C K were tested: 16, 32, 64,
    128, and 256. A learning rate combination was considered diverged or poorly-performed
    if the FID score exceeded a threshold τ, which is 50, 80, 50, 90 for CIFAR-10,
    STL-10, CelebA and LSUN-bedroom respectively. The box quartiles were plotted based
    on the cases with FID < τ while the number of diverged or poorly-performed cases
    (out of 16 learning rate combinations) was shown above each box if it is non-zero.
    We introduced τ because the diverged cases often had arbitrarily large and non-meaningful
    FID scores. DISPLAYFORM0'
  sentences:
  - An approach to perform HTN planning using external procedures to evaluate predicates
    at runtime (semantic attachments).
  - case study on optimal deep learning model for UAVs
  - Understand transferability from the perspectives of improved generalization, optimization
    and the feasibility of transferability.
pipeline_tag: sentence-similarity
library_name: sentence-transformers
---

# SentenceTransformer based on sentence-transformers/all-MiniLM-L6-v2

This is a [sentence-transformers](https://www.SBERT.net) model finetuned from [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2). It maps sentences & paragraphs to a 384-dimensional dense vector space and can be used for semantic textual similarity, semantic search, paraphrase mining, text classification, clustering, and more.

## Model Details

### Model Description
- **Model Type:** Sentence Transformer
- **Base model:** [sentence-transformers/all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) <!-- at revision c9745ed1d9f207416be6d2e6f8de32d1f16199bf -->
- **Maximum Sequence Length:** 512 tokens
- **Output Dimensionality:** 384 dimensions
- **Similarity Function:** Cosine Similarity
<!-- - **Training Dataset:** Unknown -->
<!-- - **Language:** Unknown -->
<!-- - **License:** Unknown -->

### Model Sources

- **Documentation:** [Sentence Transformers Documentation](https://sbert.net)
- **Repository:** [Sentence Transformers on GitHub](https://github.com/huggingface/sentence-transformers)
- **Hugging Face:** [Sentence Transformers on Hugging Face](https://huggingface.co/models?library=sentence-transformers)

### Full Model Architecture

```
SentenceTransformer(
  (0): Transformer({'max_seq_length': 512, 'do_lower_case': False, 'architecture': 'BertModel'})
  (1): Pooling({'word_embedding_dimension': 384, 'pooling_mode_cls_token': False, 'pooling_mode_mean_tokens': True, 'pooling_mode_max_tokens': False, 'pooling_mode_mean_sqrt_len_tokens': False, 'pooling_mode_weightedmean_tokens': False, 'pooling_mode_lasttoken': False, 'include_prompt': True})
)
```

## Usage

### Direct Usage (Sentence Transformers)

First install the Sentence Transformers library:

```bash
pip install -U sentence-transformers
```

Then you can load this model and run inference.
```python
from sentence_transformers import SentenceTransformer

# Download from the 🤗 Hub
model = SentenceTransformer("sentence_transformers_model_id")
# Run inference
sentences = [
    "Generative adversarial nets (GANs) are widely used to learn the data sampling process and their performance may heavily depend on the loss functions, given a limited computational budget. This study revisits MMD-GAN that uses the maximum mean discrepancy (MMD) as the loss function for GAN and makes two contributions. First, we argue that the existing MMD loss function may discourage the learning of fine details in data as it attempts to contract the discriminator outputs of real data. To address this issue, we propose a repulsive loss function to actively learn the difference among the real data by simply rearranging the terms in MMD. Second, inspired by the hinge loss, we propose a bounded Gaussian kernel to stabilize the training of MMD-GAN with the repulsive loss function. The proposed methods are applied to the unsupervised image generation tasks on CIFAR-10, STL-10, CelebA, and LSUN bedroom datasets. Results show that the repulsive loss function significantly improves over the MMD loss at no additional computational cost and outperforms other representative loss functions. The proposed methods achieve an FID score of 16.21 on the CIFAR-10 dataset using a single DCGAN network and spectral normalization. Generative adversarial nets (GANs) BID7 ) are a branch of generative models that learns to mimic the real data generating process. GANs have been intensively studied in recent years, with a variety of successful applications (; Li et al. (2017b);;; BID13 ). The idea of GANs is to jointly train a generator network that attempts to produce artificial samples, and a discriminator network or critic that distinguishes the generated samples from the real ones. Compared to maximum likelihood based methods, GANs tend to produce samples with sharper and more vivid details but require more efforts to train. Recent studies on improving GAN training have mainly focused on designing loss functions, network architectures and training procedures. The loss function, or simply loss, defines quantitatively the difference of discriminator outputs between real and generated samples. The gradients of loss functions are used to train the generator and discriminator. This study focuses on a loss function called maximum mean discrepancy (MMD), which is well known as the distance metric between two probability distributions and widely applied in kernel two-sample test BID8 ). Theoretically, MMD reaches its global minimum zero if and only if the two distributions are equal. Thus, MMD has been applied to compare the generated samples to real ones directly (; BID5) and extended as the loss function to the GAN framework recently (; Li et al. (2017a); ).In this paper, we interpret the optimization of MMD loss by the discriminator as a combination of attraction and repulsion processes, similar to that of linear discriminant analysis. We argue that the existing MMD loss may discourage the learning of fine details in data, as the discriminator attempts to minimize the within-group variance of its outputs for the real data. To address this issue, we propose a repulsive loss for the discriminator that explicitly explores the differences among real data. The proposed loss achieved significant improvements over the MMD loss on image generation tasks of four benchmark datasets, without incurring any additional computational cost. Furthermore, a bounded Gaussian kernel is proposed to stabilize the training of discriminator. As such, using a single kernel in MMD-GAN is sufficient, in contrast to a linear combination of kernels used in Li et al. (2017a) and. By using a single kernel, the computational cost of the MMD loss can potentially be reduced in a variety of applications. The paper is organized as follows. Section 2 reviews the GANs trained using the MMD loss (MMD-GAN). We propose the repulsive loss for discriminator in Section 3, introduce two practical techniques to stabilize the training process in Section 4, and present the of extensive experiments in Section 5. In the last section, we discuss the connections between our model and existing work. In this section, we introduce the GAN model and MMD loss. Consider a random variable X ∈ X with an empirical data distribution P X to be learned. A typical GAN model consists of two neural networks: a generator G and a discriminator D. The generator G maps a latent code z with a fixed distribution P Z (e.g., Gaussian) to the data space X: y = G(z) ∈ X, where y represents the generated samples with distribution P G. The discriminator D evaluates the scores D(a) ∈ R d of a real or generated sample a. This study focuses on image generation tasks using convolutional neural networks (CNN) for both G and D.Several loss functions have been proposed to quantify the difference of the scores between real and generated samples: {D(x)} and {D(y)}, including the minimax loss and non-saturating loss BID7 ), hinge loss , Wasserstein loss; BID10 ) and maximum mean discrepancy (MMD) (Li et al. (2017a); ) (see Appendix B.1 for more details). Among them, MMD uses kernel embedding φ(a) = k(·, a) associated with a characteristic kernel k such that φ is infinite-dimensional and φ(a), φ(b) H = k(a, b). The squared MMD distance between two distributions P and Q is DISPLAYFORM0 The kernel k(a, b) measures the similarity between two samples a and b. BID8 proved that, using a characteristic kernel k, M,P G ) reaches its minimum if and only if P X = P G (Li et al. (2017a) ). Thus, the objective functions for G and D could be (Li et al. (2017a); ): min DISPLAYFORM1 ) MMD-GAN has been shown to be more effective than the model that directly uses MMD as the loss function for the generator G (Li et al. (2017a)). showed that MMD and Wasserstein metric are weaker objective functions for GAN than the Jensen-Shannon (JS) divergence (related to minimax loss) and total variation (TV) distance (related to hinge loss). The reason is that convergence of P G to P X in JS-divergence and TV distance also implies convergence in MMD and Wasserstein metric. Weak metrics are desirable as they provide more information on adjusting the model to fit the data distribution . proved that the GAN trained using the minimax loss and gradient updates on model parameters is locally exponentially stable near equilibrium, while the GAN using Wasserstein loss is not. In Appendix A, we demonstrate that the MMD-GAN trained by gradient descent is locally exponentially stable near equilibrium. In this section, we interpret the training of MMD-GAN (using L First, consider a linear discriminant analysis (LDA) model as the discriminator. The task is to find a projection w to maximize the between-group variance w T µ x − w T µ y and minimize the withingroup variance w T (Σ x + Σ y)w, where µ and Σ are group mean and covariance. In MMD-GAN, the neural-network discriminator works in a similar way as LDA. By minimizing L att D, the discriminator D tackles two tasks: DISPLAYFORM0, causes the two groups {D(x)} and {D(y)} to repel each other (see FIG1, or maximize betweengroup variance; and 2) D increases DISPLAYFORM1 e. contracts {D(x)} and {D(y)} within each group (see FIG1, or minimize the within-group variance. We refer to loss functions that contract real data scores as attractive losses. We argue that the attractive loss L att D (Eq. 3) has two issues that may slow down the GAN training:1. The discriminator D may focus more on the similarities among real samples (in order to contract {D(x)}) than the fine details that separate them. Initially, G produces low-quality samples and it may be adequate for D to learn the common features of {x} in order to distinguish between {x} and {y}. Only when {D(y)} is sufficiently close to {D(x)} will D learn the fine details of {x} to be able to separate {D(x)} from {D(y)}. Consequently, D may leave out some fine details in real samples, thus G has no access to them during training.2. As shown in FIG1, the gradients on D(y) from the attraction (blue arrows) and repulsion (orange arrows) terms in L att D (and thus L mmd G) may have opposite directions during training. Their summation may be small in magnitude even when D(y) is far away from D(x), which may cause G to stagnate locally. Therefore, we propose a repulsive loss for D to encourage repulsion of the real data scores {D(x)}: DISPLAYFORM2 The generator G uses the same MMD loss L mmd G as before (see Eq. 2). Thus, the adversary lies in the fact that D contracts {D(y)} via maximizing FIG1 ) while G expands {D(y)} (see FIG1). Additionally, D also learns to separate the real data by minimizing DISPLAYFORM3 DISPLAYFORM4, which actively explores the fine details in real samples and may in more meaningful gradients for G. Note that in Eq. 4, D does not explicitly push the average score of {D(y)} away from that of {D(x)} because it may have no effect on the pair-wise sample distances. But G aims to match the average scores of both groups. Thus, we believe, compared to the model using DISPLAYFORM5 and L rep D is less likely to yield opposite gradients when {D(y)} and {D(x)} are distinct (see FIG1). In Appendix A, we demonstrate that GAN trained using gradient descent and the repulsive MMD loss (L At last, we identify a general form of loss function for the discriminator D: DISPLAYFORM6 In this section, we propose two approaches to stabilize the training of MMD-GAN: 1) a bounded kernel to avoid the saturation issue caused by an over-confident discriminator; and 2) a generalized power iteration method to estimate the spectral norm of a convolutional kernel, which was used in spectral normalization on the discriminator in all experiments in this study unless specified otherwise. For MMD-GAN, the following two kernels have been used:• Gaussian radial basis function (RBF), or Gaussian kernel (Li et al. (2017a) DISPLAYFORM0 2 ) where σ > 0 is the kernel scale or bandwidth.• Rational quadratic kernel DISPLAYFORM1, where the kernel scale α > 0 corresponds to a mixture of Gaussian kernels with a Gamma(α, 1) prior on the inverse kernel scales σ −1.It is interesting that both studies used a linear combination of kernels with five different kernel scales, i.e., DISPLAYFORM2, where σ i ∈ {1, 2, 4, 8, 16}, α i ∈ {0.2, 0.5, 1, 2, 5} (see FIG0 and 2c for illustration). We suspect the reason is that a single kernel k(a, b) is saturated when the distance a − b is either too large or too small compared to the kernel scale (see FIG0 and 2d), which may cause diminishing gradients during training. Both Li et al. (2017a) and applied penalties on the discriminator parameters but not to the MMD loss itself. Thus the saturation issue may still exist. Using a linear combination of kernels with different kernel scales may alleviate this issue but not eradicate it. Inspired by the hinge loss (see Appendix B.1), we propose a bounded RBF (RBF-B) kernel for the discriminator. The idea is to prevent D from pushing {D(x)} too far away from {D(y)} and causing saturation. For L att D in Eq. 3, the RBF-B kernel is: DISPLAYFORM3 For L rep D in Eq. 4, the RBF-B kernel is: DISPLAYFORM4 where b l and b u are the lower and upper bounds. As such, a single kernel is sufficient and we set σ = 1, b l = 0.25 and b u = 4 in all experiments for simplicity and leave their tuning for future work. It should be noted that, like the case of hinge loss, the RBF-B kernel is used only for the discriminator to prevent it from being over-confident. The generator is always trained using the original RBF kernel, thus we retain the interpretation of MMD loss L mmd G as a metric. RBF-B kernel is among many methods to address the saturation issue and stabilize MMD-GAN training. We found random sampling kernel scale, instance noise (Sønderby et al. ) and label smoothing may also improve the model performance and stability. However, the computational cost of RBF-B kernel is relatively low. Without any Lipschitz constraints, the discriminator D may simply increase the magnitude of its outputs to minimize the discriminator loss, causing unstable training 3. Spectral normalization divides the weight matrix of each layer by its spectral norm, which imposes an upper bound on the magnitudes of outputs and gradients at each layer of D . However, to estimate the spectral norm of a convolution kernel, reshaped the kernel into a matrix. We propose a generalized power iteration method to directly estimate the spectral norm of a convolution kernel (see Appendix C for details) and applied spectral normalization to the discriminator in all experiments. In Appendix D.1, we explore using gradient penalty to impose the Lipschitz constraint BID10;; ) for the proposed repulsive loss. In this section, we empirically evaluate the proposed 1) repulsive loss L (Li et al. (2017a) ) and rational quadratic kernel (MMD-rq) ), as well as non-saturating loss BID7 ) and hinge loss . To show the efficacy of RBF-B kernel, we applied it to both L ). DISPLAYFORM0 Dataset: The loss functions were evaluated on four datasets: 1) CIFAR-10 (50K images, 32 × 32 pixels) ; 2) STL-10 (100K images, 48 × 48 pixels) BID3 ); 3) CelebA (about 203K images, 64 × 64 pixels) (Liu et al. FORMULA8); and 4) LSUN bedrooms (around 3 million images, 64×64 pixels) (Yu et al. FORMULA8). The images were scaled to range [−1, 1] to avoid numeric issues. FORMULA8 ) was used in the generator, and spectral normalization with the generalized power iteration (see Appendix C) in the discriminator. For MMD related losses, the dimension of discriminator output layer was set to 16; for non-saturating loss and hinge loss, it was 1. In Appendix D.2, we investigate the impact of discriminator output dimension on the performance of repulsive loss. ) and thus omitted.3 On LSUN-bedroom, MMD-rbf and MMD-rq did not achieve reasonable and thus are omitted. Hyper-parameters: We used Adam optimizer (Kingma & Ba FORMULA8) with momentum parameters β 1 = 0.5, β 2 = 0.999; two-timescale update rule (TTUR) BID12 ) with two learning rates (ρ D, ρ G) chosen from {1e-4, 2e-4, 5e-4, 1e-3} (16 combinations in total); and batch size 64. Fine-tuning on learning rates may improve the model performance, but constant learning rates were used for simplicity. All models were trained for 100K iterations on CIFAR-10, STL-10, CelebA and LSUN bedroom datasets, with n dis = 1, i.e., one discriminator update per generator update 4. For MMD-rbf, the kernel scales σ i ∈ {1, √ 2, 2, 2 √ 2, 4} were used due to a better performance than the original values used in Li et al. (2017a). For MMD-rq, α i ∈ {0.2, 0.5, 1, 2, 5}. For MMD-rbf-b, MMD-rep, MMD-rep-b, a single Gaussian kernel with σ = 1 was used. Evaluation metrics: Inception score (IS) , Fréchet Inception distance (FID) BID12 ) and multi-scale structural similarity (MS-SSIM) were used for quantitative evaluation. Both IS and FID are calculated using a pre-trained Inception model . Higher IS and lower FID scores indicate better image quality. MS-SSIM calculates the pair-wise image similarity and is used to detect mode collapses among images of the same class . Lower MS-SSIM values indicate perceptually more diverse images. For each model, 50K randomly generated samples and 50K real samples were used to calculate IS, FID and MS-SSIM. TAB0 shows the Inception score, FID and MS-SSIM of applying different loss functions on the benchmark datasets with the optimal learning rate combinations tested experimentally. Note that the same training setup (i.e., DCGAN + BN + SN + TTUR) was applied for each loss function. We observed that: 1) MMD-rep and MMD-rep-b performed significantly better than MMD-rbf and MMD-rbf-b respectively, showing the proposed repulsive loss L rep D (Eq. 4) greatly improved over the attractive loss L att D (Eq. 3); 2) Using a single kernel, MMD-rbf-b performed better than MMD-rbf and MMD-rq which used a linear combination of five kernels, indicating that the kernel saturation may be an issue that slows down MMD-GAN training; 3) MMD-rep-b performed comparable or better than MMD-rep on benchmark datasets where we found the RBF-B kernel managed to stabilize MMD-GAN training using repulsive loss. 4) MMD-rep and MMD-rep-b performed significantly better than the non-saturating and hinge losses, showing the efficacy of the proposed repulsive loss. Additionally, we trained MMD-GAN using the general loss L D,λ (Eq. 5) for discriminator and L mmd G (Eq. 2) for generator on the CIFAR-10 dataset. Each color bar represents the FID score using a learning rate combination (ρ D, ρ G), in the order of (1e-4, 1e-4), (1e-4, 2e-4),...,(1e-3, 1e-3). The discriminator was trained using L D,λ (Eq. 5) with λ ∈ {-1, -0.5, 0, 0.5, 1, 2}, and generator using L mmd G (Eq. 2). We use the FID> 30 to indicate that the model diverged or produced poor .of MMD-GAN with RBF and RBF-B kernel 5. Note that when λ = −1, the models are essentially MMD-rbf (with a single Gaussian kernel) and MMD-rbf-b when RBF and RBF-B kernel are used respectively. We observed that: 1) the model performed well using repulsive loss (i.e., λ ≥ 0), with λ = 0.5, 1 slightly better than λ = −0.5, 0, 2; 2) the MMD-rbf model can be significantly improved by simply increasing λ from −1 to −0.5, which reduces the attraction of discriminator on real sample scores; 3) larger λ may lead to more diverged models, possibly because the discriminator focuses more on expanding the real sample scores over adversarial learning; note when λ 1, the model would simply learn to expand all real sample scores and pull the generated sample scores to real samples', which is a divergent process; 4) the RBF-B kernel managed to stabilize MMD-rep for most diverged cases but may occasionally cause the FID score to rise up. The proposed methods were further evaluated in Appendix A, C and D. In Appendix A.2, we used a simulation study to show the local stability of MMD-rep trained by gradient descent, while its global stability is not guaranteed as bad initialization may lead to trivial solutions. The problem may be alleviated by adjusting the learning rate for generator. In Appendix C.3, we showed the proposed generalized power iteration (Section 4.2) imposes a stronger Lipschitz constraint than the method in , and benefited MMD-GAN training using the repulsive loss. Moreover, the RBF-B kernel managed to stabilize the MMD-GAN training for various configurations of the spectral normalization method. In Appendix D.1, we showed the gradient penalty can also be used with the repulsive loss. In Appendix D.2, we showed that it was better to use more than one neuron at the discriminator output layer for the repulsive loss. The discriminator outputs may be interpreted as a learned representation of the input samples. FIG8 visualizes the discriminator outputs learned by the MMD-rbf and proposed MMD-rep methods on CIFAR-10 dataset using t-SNE (van der Maaten FORMULA4). MMD-rbf ignored the class structure in data (see FIG8) while MMD-rep learned to concentrate the data from the same class and separate different classes to some extent FIG8. This is because the discriminator D has to actively learn the data structure in order to expands the real sample scores {D(x)}. Thus, we speculate that techniques reinforcing the learning of cluster structures in data may further improve the training of MMD-GAN.In addition, the performance gain of proposed repulsive loss (Eq. 4) over the attractive loss (Eq. 3) comes at no additional computational cost. In fact, by using a single kernel rather than a linear combination of kernels, MMD-rep and MMD-rep-b are simpler than MMD-rbf and MMD-rq. Besides, given a typically small batch size and a small number of discriminator output neurons (64 and 16 in our experiments), the cost of MMD over the non-saturating and hinge loss is marginal compared to the convolution operations. In Appendix D.3, we provide some random samples generated by the methods in our study. This study extends the previous work on MMD-GAN (Li et al. (2017a) ) with two contributions. First, we interpreted the optimization of MMD loss as a combination of attraction and repulsion processes, and proposed a repulsive loss for the discriminator that actively learns the difference among real data. Second, we proposed a bounded Gaussian RBF (RBF-B) kernel to address the saturation issue. Empirically, we observed that the repulsive loss may in unstable training, due to factors including initialization (Appendix A.2), learning rate (FIG7 and Lipschitz constraints on the discriminator (Appendix C.3). The RBF-B kernel managed to stabilize the MMD-GAN training in many cases. Tuning the hyper-parameters in RBF-B kernel or using other regularization methods may further improve our . The theoretical advantages of MMD-GAN require the discriminator to be injective. The proposed repulsive loss (Eq. 4) attempts to realize this by explicitly maximizing the pair-wise distances among the real samples. Li et al. (2017a) achieved the injection property by using the discriminator as the encoder and an auxiliary network as the decoder to reconstruct the real and generated samples, which is more computationally extensive than our proposed approach. On the other hand,; imposed a Lipschitz constraint on the discriminator in MMD-GAN via gradient penalty, which may not necessarily promote an injective discriminator. The idea of repulsion on real sample scores is in line with existing studies. It has been widely accepted that the quality of generated samples can be significantly improved by integrating labels (; ;) or even pseudo-labels generated by k-means method BID9 ) in the training of discriminator. The reason may be that the labels help concentrate the data from the same class and separate those from different classes. Using a pre-trained classifier may also help produce vivid image samples BID14 ) as the learned representations of the real samples in the hidden layers of the classifier tend to be well separated/organized and may produce more meaningful gradients to the generator. At last, we note that the proposed repulsive loss is orthogonal to the GAN studies on designing network structures and training procedures, and thus may be combined with a variety of novel techniques. For example, the ResNet architecture BID11 ) has been reported to outperform the plain DCGAN used in our experiments on image generation tasks (; BID10) and self-attention module may further improve the . On the other hand, proposed to progressively grows the size of both discriminator and generator and achieved the state-of-the-art performance on unsupervised training of GANs on the CIFAR-10 dataset. Future work may explore these directions. This section demonstrates that, under mild assumptions, MMD-GAN trained by gradient descent is locally exponentially stable at equilibrium. It is organized as follows. The main assumption and proposition are presented in Section A.1, followed by simulation study in Section A.2 and proof in Section A.3. We discuss the indications of assumptions on the discriminator of GAN in Section A.4. We consider GAN trained using the MMD loss L DISPLAYFORM0 where Thus in contrast to Assumption 1, we assume Assumption 2. For GANs using MMD loss in Eq. S1, and random initialization on parameters, at equilibrium, DISPLAYFORM1 DISPLAYFORM2 is not constant almost everywhere. We use a simulation study in Section A.2 to show that D θ * D (x) = 0 does not hold in general for MMD loss. Based on Assumption 2, we propose the following proposition and prove it in Appendix A.3: Proposition 1. If there exists θ * G ∈ Θ G such that P θ * G = P X, then GANs with MMD loss in Eq. S1 has equilibria (θ * G, θ D) for any θ D ∈ Θ D. Moreover, the model trained using gradient descent methods is locally exponentially stable at (θ * DISPLAYFORM3 There may exist non-realizable cases where the mapping between P Z and P X cannot be represented by any generator G θ G with θ G ∈ Θ G . In Section A.2, we use a simulation study to show that both the attractive MMD loss L att D (Eq. S1b) and the proposed repulsive loss L rep D (Eq. S1c) may be locally stable and leave the proof for future work. In this section, we reused the example from to show that GAN trained using the MMD loss in Eq. S1 is locally stable. Consider a two-parameter MMD-GAN with uniform latent distribution P Z over [−1, 1], generator G(z) = w 1 z, discriminator D(x) = w 2 x 2, and Gaussian kernel k (a) the data distribution P X is the same as P Z, i.e., uniform over [−1, 1], thus P X is realizable; DISPLAYFORM0 Figure S1: Streamline plots of MMD-GAN using the MMD-rbf and the MMD-rep model on distributions: P Z = U(−1, 1), P X = U(−1, 1) or P X = N. In (a) and (b), the equilibria satisfying P G = P X lie on the line w 1 = 1. In (c), the equilibrium lies around point (1.55, 0.74); in (d), it is around (1.55, 0.32).(b) P X is standard Gaussian, thus non-realizable for any w 1 ∈ R. FIG1 shows that MMD-GAN are locally stable in both cases and D θ * D (x) = 0 does not hold in general for MMD loss. However, MMD-rep may not be globally stable for the tested cases: initialization of (w 1, w 2) in some regions may lead to the trivial solution w 2 = 0 (see FIG1 and S1d). We note that by decreasing the learning rate for G, the area of such regions decreased. At last, it is interesting to note that both MMD-rbf and MMD-rep had the same nontrivial solution w 1 ≈ 1.55 for generator in the non-realizable cases (see FIG1 and S1d). This section divides the proof for Proposition 1 into two parts. First, we show that GAN with the MMD loss in Eq. S1 has equilibria for any parameter configuration of discriminator D; second, we prove the model is locally exponentially stable. For convenience, we consider the general form of discriminator loss in Eq. 5: DISPLAYFORM0 which has L att D and L rep D as the special cases when λ equals −1 and 1 respectively. Consider real data X r ∼ P X, latent variable Z ∼ P Z and generated variable Y g = G θ G (Z). Let x r, z, y g be their samples. Denote ∇. DISPLAYFORM1 where L D and L G are the losses for D and G respectively. Assume an isotropic stationary kernel k(a, b) = k I (a − b) BID6 ) is used in MMD. We first show:Proposition 1 (Part 1). If there exists θ * G ∈ Θ G such that P θ * G = P X, the GAN with the MMD loss in Eq. S1a and Eq. S2 has equilibria (θ * DISPLAYFORM2 where k is the kernel of MMD. The gradients of MMD loss are DISPLAYFORM3 ∼ P G, an unbiased estimator of the squared MMD is BID8) We proceed to prove the model stability. First, following Theorem 5 in BID8 and Theorem 4 in Li et al. (2017a), it is straightforward to see: FORMULA13 ). Consider a non-linear system of parameters (θ, γ): θ = h 1 (θ, γ),γ = h 2 (θ, γ) with an equilibrium point at. Let there exist such that ∀γ ∈ DISPLAYFORM4 DISPLAYFORM5 DISPLAYFORM6 is a Hurwitz matrix, the non-linear system is exponentially stable. Proposition 1 (Part 2). At equilibrium P θ * G = P X, the GAN trained using MMD loss and gradient descent methods is locally exponentially stable at (θ DISPLAYFORM0 ∂b∂c . Based on Eq. S3, we have DISPLAYFORM1 where ⊗ is the kronecker product. At equilibrium, consider a sequence of N samples DISPLAYFORM2 DISPLAYFORM3 Given Lemma A.1 and fact that J GG is the Hessian matrix of M DISPLAYFORM4 is locally constant along some directions in the parameter space of G. As a , null(J GG) ⊆ null(J DG) because varying θ * G along these directions has no effect on D. Following Lemma C.3 of , we consider eigenvalue decomposition DISPLAYFORM5. Thus, the projections γ G = T G θ G are orthogonal to null(J GG). Then, the Jacobian corresponding to the projected system has the form DISPLAYFORM6, where J GG is negative definite. Moreover, on all directions exclude those described by J GG, the system is surrounded by a neighborhood of equilibia at least locally. According to Lemma A.2, the system is exponentially stable. This section shows that constant discriminator output DISPLAYFORM0 may have no discrimination power. First, we make the following assumptions:Assumption 3. 1. D is a multilayer perceptron where each layer l can be factorized into an affine transform and an element-wise activation function f l. 2. Each activation function f l ∈ C 0; furthermore, f l has a finite number of discontinuities and f l ∈ C 06. 3. Input data to D is continuous and its support S is compact in R d with non-zero measure in each dimension and d > 1 7.Based on Assumption 3, we have the following proposition:Proposition 2. If ∀x ∈ S, D(x) = c, where c is constant, then there always exists distortion δx such that x + δx ∈ S and D(x + δx) = c. are model weights and biases, f is an activation function satisfying Assumption 3. For x ∈ S, since D(x) = c, we have h(x) ∈ null(W 2). Furthermore: DISPLAYFORM0 has unique solution for any k ∈ R as long as k · h(x) is within the output range of f. DISPLAYFORM1 and n is the nullity of W 2. Let the projected support beŜ. Thus, DISPLAYFORM2 T with z c = 0.Consider the Jacobian: DISPLAYFORM3 where DISPLAYFORM4 is the input to activation, or pre-activations. SinceŜ is continuous and compact, it has infinite number of boundary points {x b} for d > 1. Consider one boundary pointx b and its normal line δx b. Let > 0 be a small scalar such thatx b − δx b ∈ S andx b + δx b ∈Ŝ.• For linear activation, ∇Σ = I and J is constant. Then z c remains 0 DISPLAYFORM5 there exists z such that h(x + δx) ∈ null(W 2).• For nonlinear activations, assume f has N discontinuities. Since U x T 0 T T + b 1 = c has unique solution for any vector c, the boundary points {x b} cannot yield pre-activations {a b} that all lie on the discontinuities in any of the d h directions. Though we might need to sample d N +1 h points in the worst case to find an exception, there are infinite number of exceptions. Letx b be a sample where {a b} does not lie on the discontinuities in any direction. Because f is continuous, z c remains 0 forx b + δx b, i.e., there exists z such that h(x + δx) ∈ null(W 2).In , we can always find δx such that x + δx / ∈ S and D(x + δx) = c. cannot discriminate against fake samples with distortions to the original data. In contrast, Assumption 2 and Lemma A.1 guarantee that, at equilibrium, the discriminator trained using MMD loss function is effective against such fake samples given a large number of i.i.d. test samples BID8 ). Several loss functions have been proposed to quantify the difference between real and generated sample scores, including: (assume linear activation is used at the last layer of D)• The Minimax loss BID7 ): Softplus(D(G(z) ))] and L G = −L D, which can be derived from the Jensen-Shannon (JS) divergence between P X and the model distribution P G. DISPLAYFORM0 • The non-saturating loss BID7 ), which is a variant of the minimax loss with the same L D and DISPLAYFORM1 • The Hinge loss : DISPLAYFORM2, which is notably known for usage in support vector machines and is related to the total variation (TV) distance .• The Wasserstein loss; BID10 ), which is derived from the Wasserstein distance between P X and P G: DISPLAYFORM3, where D is subject to some Lipschitz constraint.• The maximum mean discrepancy (MMD) (Li et al. (2017a); ), as described in Section 2. For unsupervised image generation tasks on CIFAR-10 and STL-10 datasets, the DCGAN architecture from was used. For CelebA and LSUN bedroom datasets, we added more layers to the generator and discriminator accordingly. See TAB0 and S2 for details. TAB0: DCGAN models for image generation on CIFAR-10 (h = w = 4, H = W = 32) and STL-10 (h = w = 6, H = W = 48) datasets. For non-saturating loss and hinge loss, s = 1; for MMD-rand, MMD-rbf, MMD-rq, s = 16. For a weight matrix W, the spectral norm is defined as σ(W) = max v 2 ≤1 W v 2. The PIM is used to estimate σ(W) , which iterates between two steps: DISPLAYFORM0 The convolutional kernel W c is a tensor of shape h × w × c in × c out with h, w the receptive field size and c in, c out the number of input/output channels. To estimate σ(W c), reshaped it into a matrix W rs of shape (hwc in) × c out and estimated σ(W rs).We propose a simple method to calculate W c directly based on the fact that convolution operation is linear. For any linear map T: R m → R n, there exists matrix W L ∈ R n×m such that y = T (x) can be represented as y = W L x. Thus, we may simply substitute W L = ∂y ∂x in the PIM method to estimate the spectral norm of any linear operation. In the case of convolution operation *, there exist doubly block circulant matrix DISPLAYFORM1 T u which is essentially the transpose convolution of W c on u BID4 ). Thus, similar to PIM, PICO iterates between the following two steps: DISPLAYFORM2 2. Do transpose convolution of W c on u to getv; update v =v/ v 2.Similar approaches have been proposed in and from different angles, which we were not aware during this study. In addition, proposes to compute the exact singular values of convolution kernels using FFT and SVD. In spectral normalization, only the first singular value is concerned, making the power iteration methods PIM and PICO more efficient than FFT and thus preferred in our study. However, we believe the exact method FFT+SVD may eventually inspire more rigorous regularization methods for GAN.The proposed PICO method estimates the real spectral norm of a convolution kernel at each layer, thus enforces an upper bound on the Lipschitz constant of the discriminator D. Denote the upper bound as LIP PICO. In this study, Leaky ReLU (LReLU) was used at each layer of D, thus LIP PICO ≈ 1 . In practice, however, PICO would often cause the norm of the signal passing through D to decrease to zero, because at each layer,• the signal hardly coincides with the first singular-vector of the convolution kernel; and• the activation function LReLU often reduces the norm of the signal. Consequently, the discriminator outputs tend to be similar for all the inputs. To compensate the loss of norm at each layer, the signal is multiplied by a constant C after each spectral normalization. This essentially enlarges LIP PICO by C K where K is the number of layers in the DCGAN discriminator. For all experiments in Section 5, we fixed C = 1 0.55 ≈ 1.82 as all loss functions performed relatively well empirically. In Appendix Section C.3, we tested the effects of coefficient C K on the performance of several loss functions. PIM also enforces an upper bound LIP PIM on the Lipschitz constant of the discriminator D. Consider a convolution kernel W c with receptive field size h × w and stride s. Let σ PICO and σ PIM be the spectral norm estimated by PICO and PIM respectively. We empirically In this section, we empirically evaluate the effects of coefficient C K on the performance of PICO and compare PICO against PIM using several loss functions. We used a similar setup as Section 5.1 with the following adjustments. Four loss functions were tested: hinge, MMD-rbf, MMD-rep and MMD-rep-b. Either PICO or PIM was used at each layer of the discriminator. For PICO, five coefficients C K were tested: 16, 32, 64, 128 and 256 (note this is the overall coefficient for K layers; K = 8 for CIFAR-10 and STL-10; K = 10 for CelebA and LSUN-bedroom; see Appendix B.2). FID was used to evaluate the performance of each combination of loss function and power iteration method, e.g., hinge + PICO with C K = 16.Results: For each combination of loss function and power iteration method, the distribution of FID scores over 16 learning rate combinations is shown in FIG0. We separated well-performed learning rate combinations from diverged or poorly-performed ones using a threshold τ as the diverged cases often had non-meaningful FID scores. The boxplot shows the distribution of FID scores for goodperformed cases while the number of diverged or poorly-performed cases was shown above each box if it is non-zero. 1) When PICO was used, the hinge, MMD-rbf and MMD-rep methods were sensitive to the choices of C K while MMD-rep-b was robust. For hinge and MMD-rbf, higher C K may in better FID scores and less diverged cases over 16 learning rate combinations. For MMD-rep, higher C K may cause more diverged cases; however, the best FID scores were often achieved with C K = 64 or 128.2) For CIFAR-10, STL-10 and CelebA datasets, PIM performed comparable to PICO with C K = 128 or 256 on four loss functions. For LSUN bedroom dataset, it is likely that the performance of PIM corresponded to that of PICO with C K > 256. This implies that PIM may in a relatively loose Lipschitz constraint on deep convolutional networks.3) MMD-rep-b performed generally better than hinge and MMD-rbf with tested power iteration methods and hyper-parameter configurations. Using PICO, MMD-rep also achieved generally better FID scores than hinge and MMD-rbf. This implies that, given a limited computational budget, the proposed repulsive loss may be a better choice than the hinge and MMD loss for the discriminator. TAB2 shows the best FID scores obtained by PICO and PIM where C K was fixed at 128 for hinge and MMD-rbf, and 64 for MMD-rep and MMD-rep-b. For hinge and MMD-rbf, PICO performed significantly better than PIM on the LSUN-bedroom dataset and comparably on the rest datasets. For MMD-rep and MMD-rep-b, PICO achieved consistently better FID scores than PIM.However, compared to PIM, PICO has a higher computational cost which roughly equals the additional cost incurred by increasing the batch size by two . This may be problematic when a small batch has to be used due to memory constraints, e.g., when handling high resolution images on a single GPU. Thus, we recommend using PICO when the computational cost is less of a concern. D SUPPLEMENTARY EXPERIMENTS D.1 LIPSCHITZ CONSTRAINT VIA GRADIENT PENALTY Gradient penalty has been widely used to impose the Lipschitz constraint on the discriminator arguably since Wasserstein GAN BID10 ). This section explores whether the proposed repulsive loss can be applied with gradient penalty. Several gradient penalty methods have been proposed for MMD-GAN. penalized the gradient norm of witness function y) ] w.r.t. the interpolated sample z = ux + (1 − u)y to one, where u ∼ U 9. More recently, proposed to impose the Lipschitz constraint on the mapping φ • D directly and derived the Scaled MMD (SMMD) as SM k (P, Q) = σ µ,k,λ M k (P, Q), where the scale σ µ,k,λ incorporates gradient and smooth penalties. Using the Gaussian kernel and measure µ = P X leads to the discriminator loss: DISPLAYFORM0 DISPLAYFORM1 We apply the same formation of gradient penalty to the repulsive loss: DISPLAYFORM2 where the numerator L rep D − 1 ≤ 0 so that the discriminator will always attempt to minimize both L rep D and the Frobenius norm of gradients ∇D(x) w.r.t. real samples. Meanwhile, the generator is trained using the MMD loss L mmd G (Eq. 2).Experiment setup: The gradient-penalized repulsive loss L rep-gp D (Eq. S8, referred to as MMD-repgp) was evaluated on the CIFAR-10 dataset. We found λ = 10 in too restrictive 9 Empirically, we found this gradient penalty did not work with the repulsive loss. The reason may be the attractive loss L att D (Eq. 3) is symmetric in the sense that swapping P X and PG in the same loss; while the repulsive loss is asymmetric and naturally in varying gradient norms in data space. and used λ = 0.1 instead. Same as, the output dimension of discriminator was set to one. Since we entrusted the Lipschitz constraint to the gradient penalty, spectral normalization was not used. The rest experiment setup can be found in Section 5.1. TAB3 shows that the proposed repulsive loss can be used with gradient penalty to achieve reasonable on CIFAR-10 dataset. For comparison, we cited the Inception score and FID for Scaled MMD-GAN (SMMDGAN) and Scaled MMD-GAN with spectral normalization (SN-SMMDGAN) from. Note that SMMDGAN and SN-SMMDGAN used the same DCGAN architecture as MMD-rep-gp, but were trained for 150k generator updates and 750k discriminator updates, much more than that of MMD-rep-gp (100k for both G and D). Thus, the repulsive loss significantly improved over the attractive MMD loss for discriminator. In this section, we investigate the impact of the output dimension of discriminator on the performance of repulsive loss. Experiment setup: We used a similar setup as Section 5.1 with the following adjustments. The repulsive loss was tested on the CIFAR-10 dataset with a variety of discriminator output dimensions: d ∈ {1, 4, 16, 64, 256}. Spectral normalization was applied to discriminator with the proposed PICO method (see Appendix C) and the coefficients C K selected from {16, 32, 64, 128, 256}.Results: TAB4 shows that using more than one output neuron in the discriminator D significantly improved the performance of repulsive loss over the one-neuron case on CIFAR-10 dataset. The reason may be that using insufficient output neurons makes it harder for the discriminator to learn an injective and discriminative representation of the data (see FIG8). However, the performance gain diminished when more neurons were used, perhaps because it becomes easier for D to surpass the generator G and trap it around saddle solutions. The computation cost also slightly increased due to more output neurons. Generated samples on CelebA dataset are given in FIG7 and LSUN bedrooms in FIG8. Spectral normalization was applied to discriminator with two power iteration methods: PICO and PIM. For PICO, five coefficients C K were tested: 16, 32, 64, 128, and 256. A learning rate combination was considered diverged or poorly-performed if the FID score exceeded a threshold τ, which is 50, 80, 50, 90 for CIFAR-10, STL-10, CelebA and LSUN-bedroom respectively. The box quartiles were plotted based on the cases with FID < τ while the number of diverged or poorly-performed cases (out of 16 learning rate combinations) was shown above each box if it is non-zero. We introduced τ because the diverged cases often had arbitrarily large and non-meaningful FID scores. DISPLAYFORM0",
    'case study on optimal deep learning model for UAVs',
    'Understand transferability from the perspectives of improved generalization, optimization and the feasibility of transferability.',
]
embeddings = model.encode(sentences)
print(embeddings.shape)
# [3, 384]

# Get the similarity scores for the embeddings
similarities = model.similarity(embeddings, embeddings)
print(similarities)
# tensor([[1.0000, 0.1222, 0.1636],
#         [0.1222, 1.0000, 0.1692],
#         [0.1636, 0.1692, 1.0000]])
```

<!--
### Direct Usage (Transformers)

<details><summary>Click to see the direct usage in Transformers</summary>

</details>
-->

<!--
### Downstream Usage (Sentence Transformers)

You can finetune this model on your own dataset.

<details><summary>Click to expand</summary>

</details>
-->

<!--
### Out-of-Scope Use

*List how the model may foreseeably be misused and address what users ought not to do with the model.*
-->

<!--
## Bias, Risks and Limitations

*What are the known or foreseeable issues stemming from this model? You could also flag here known failure cases or weaknesses of the model.*
-->

<!--
### Recommendations

*What are recommendations with respect to the foreseeable issues? For example, filtering explicit content.*
-->

## Training Details

### Training Dataset

#### Unnamed Dataset

* Size: 1,599 training samples
* Columns: <code>sentence_0</code>, <code>sentence_1</code>, and <code>label</code>
* Approximate statistics based on the first 1000 samples:
  |         | sentence_0                                                                            | sentence_1                                                                        | label                                                          |
  |:--------|:--------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------|:---------------------------------------------------------------|
  | type    | string                                                                                | string                                                                            | float                                                          |
  | details | <ul><li>min: 101 tokens</li><li>mean: 504.95 tokens</li><li>max: 512 tokens</li></ul> | <ul><li>min: 5 tokens</li><li>mean: 27.96 tokens</li><li>max: 62 tokens</li></ul> | <ul><li>min: 0.0</li><li>mean: 0.51</li><li>max: 1.0</li></ul> |
* Samples:
  | sentence_0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | sentence_1                                                                                                                                                           | label            |
  |:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------|
  | <code>Weight-sharing—the simultaneous optimization of multiple neural networks using the same parameters—has emerged as a key component of state-of-the-art neural architecture search. However, its success is poorly understood and often found to be surprising. We argue that, rather than just being an optimization trick, the weight-sharing approach is induced by the relaxation of a structured hypothesis space, and introduces new algorithmic and theoretical challenges as well as applications beyond neural architecture search. Algorithmically, we show how the geometry of ERM for weight-sharing requires greater care when designing gradient- based minimization methods and apply tools from non-convex non-Euclidean optimization to give general-purpose algorithms that adapt to the underlying structure. We further analyze the learning-theoretic behavior of the bilevel optimization solved by practical weight-sharing methods. Next, using kernel configuration and NLP feature selection as case studies, we...</code> | <code>An analysis of the learning and optimization structures of architecture search in neural networks and beyond.</code>                                           | <code>1.0</code> |
  | <code>Reinforcement learning provides a powerful and general framework for decision making and control, but its application in practice is often hindered by the need for extensive feature and reward engineering. Deep reinforcement learning methods can remove the need for explicit engineering of policy or value features, but still require a manually specified reward function. Inverse reinforcement learning holds the promise of automatic reward acquisition, but has proven exceptionally difficult to apply to large, high-dimensional problems with unknown dynamics. In this work, we propose AIRL, a practical and scalable inverse reinforcement learning algorithm based on an adversarial reward learning formulation that is competitive with direct imitation learning algorithms. Additionally, we show that AIRL is able to recover portable reward functions that are robust to changes in dynamics, enabling us to learn policies even under significant variation in the environment seen during training. While ...</code> | <code>We propose an adversarial inverse reinforcement learning algorithm capable of learning reward functions which can transfer to new, unseen environments.</code> | <code>1.0</code> |
  | <code>Compression is a key step to deploy large neural networks on resource-constrained platforms. As a popular compression technique, quantization constrains the number of distinct weight values and thus reducing the number of bits required to represent and store each weight. In this paper, we study the representation power of quantized neural networks. First, we prove the universal approximability of quantized ReLU networks on a wide class of functions. Then we provide upper bounds on the number of weights and the memory size for a given approximation error bound and the bit-width of weights for function-independent and function-dependent structures. Our reveal that, to attain an approximation error bound of $\epsilon$, the number of weights needed by a quantized network is no more than $\mathcal{O}\left(\log^5(1/\epsilon)\right)$ times that of an unquantized network. This overhead is of much lower order than the lower bound of the number of weights needed for the error bound, supporting t...</code> | <code>This paper proves the universal  approximability of quantized ReLU neural networks and puts forward the complexity bound given arbitrary error.</code>         | <code>1.0</code> |
* Loss: [<code>CosineSimilarityLoss</code>](https://sbert.net/docs/package_reference/sentence_transformer/losses.html#cosinesimilarityloss) with these parameters:
  ```json
  {
      "loss_fct": "torch.nn.modules.loss.MSELoss"
  }
  ```

### Training Hyperparameters
#### Non-Default Hyperparameters

- `per_device_train_batch_size`: 16
- `per_device_eval_batch_size`: 16
- `multi_dataset_batch_sampler`: round_robin

#### All Hyperparameters
<details><summary>Click to expand</summary>

- `overwrite_output_dir`: False
- `do_predict`: False
- `eval_strategy`: no
- `prediction_loss_only`: True
- `per_device_train_batch_size`: 16
- `per_device_eval_batch_size`: 16
- `per_gpu_train_batch_size`: None
- `per_gpu_eval_batch_size`: None
- `gradient_accumulation_steps`: 1
- `eval_accumulation_steps`: None
- `torch_empty_cache_steps`: None
- `learning_rate`: 5e-05
- `weight_decay`: 0.0
- `adam_beta1`: 0.9
- `adam_beta2`: 0.999
- `adam_epsilon`: 1e-08
- `max_grad_norm`: 1
- `num_train_epochs`: 3
- `max_steps`: -1
- `lr_scheduler_type`: linear
- `lr_scheduler_kwargs`: {}
- `warmup_ratio`: 0.0
- `warmup_steps`: 0
- `log_level`: passive
- `log_level_replica`: warning
- `log_on_each_node`: True
- `logging_nan_inf_filter`: True
- `save_safetensors`: True
- `save_on_each_node`: False
- `save_only_model`: False
- `restore_callback_states_from_checkpoint`: False
- `no_cuda`: False
- `use_cpu`: False
- `use_mps_device`: False
- `seed`: 42
- `data_seed`: None
- `jit_mode_eval`: False
- `bf16`: False
- `fp16`: False
- `fp16_opt_level`: O1
- `half_precision_backend`: auto
- `bf16_full_eval`: False
- `fp16_full_eval`: False
- `tf32`: None
- `local_rank`: 0
- `ddp_backend`: None
- `tpu_num_cores`: None
- `tpu_metrics_debug`: False
- `debug`: []
- `dataloader_drop_last`: False
- `dataloader_num_workers`: 0
- `dataloader_prefetch_factor`: None
- `past_index`: -1
- `disable_tqdm`: False
- `remove_unused_columns`: True
- `label_names`: None
- `load_best_model_at_end`: False
- `ignore_data_skip`: False
- `fsdp`: []
- `fsdp_min_num_params`: 0
- `fsdp_config`: {'min_num_params': 0, 'xla': False, 'xla_fsdp_v2': False, 'xla_fsdp_grad_ckpt': False}
- `fsdp_transformer_layer_cls_to_wrap`: None
- `accelerator_config`: {'split_batches': False, 'dispatch_batches': None, 'even_batches': True, 'use_seedable_sampler': True, 'non_blocking': False, 'gradient_accumulation_kwargs': None}
- `parallelism_config`: None
- `deepspeed`: None
- `label_smoothing_factor`: 0.0
- `optim`: adamw_torch_fused
- `optim_args`: None
- `adafactor`: False
- `group_by_length`: False
- `length_column_name`: length
- `project`: huggingface
- `trackio_space_id`: trackio
- `ddp_find_unused_parameters`: None
- `ddp_bucket_cap_mb`: None
- `ddp_broadcast_buffers`: False
- `dataloader_pin_memory`: True
- `dataloader_persistent_workers`: False
- `skip_memory_metrics`: True
- `use_legacy_prediction_loop`: False
- `push_to_hub`: False
- `resume_from_checkpoint`: None
- `hub_model_id`: None
- `hub_strategy`: every_save
- `hub_private_repo`: None
- `hub_always_push`: False
- `hub_revision`: None
- `gradient_checkpointing`: False
- `gradient_checkpointing_kwargs`: None
- `include_inputs_for_metrics`: False
- `include_for_metrics`: []
- `eval_do_concat_batches`: True
- `fp16_backend`: auto
- `push_to_hub_model_id`: None
- `push_to_hub_organization`: None
- `mp_parameters`: 
- `auto_find_batch_size`: False
- `full_determinism`: False
- `torchdynamo`: None
- `ray_scope`: last
- `ddp_timeout`: 1800
- `torch_compile`: False
- `torch_compile_backend`: None
- `torch_compile_mode`: None
- `include_tokens_per_second`: False
- `include_num_input_tokens_seen`: no
- `neftune_noise_alpha`: None
- `optim_target_modules`: None
- `batch_eval_metrics`: False
- `eval_on_start`: False
- `use_liger_kernel`: False
- `liger_kernel_config`: None
- `eval_use_gather_object`: False
- `average_tokens_across_devices`: True
- `prompts`: None
- `batch_sampler`: batch_sampler
- `multi_dataset_batch_sampler`: round_robin
- `router_mapping`: {}
- `learning_rate_mapping`: {}

</details>

### Framework Versions
- Python: 3.12.12
- Sentence Transformers: 5.1.2
- Transformers: 4.57.1
- PyTorch: 2.8.0+cu126
- Accelerate: 1.11.0
- Datasets: 4.0.0
- Tokenizers: 0.22.1

## Citation

### BibTeX

#### Sentence Transformers
```bibtex
@inproceedings{reimers-2019-sentence-bert,
    title = "Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks",
    author = "Reimers, Nils and Gurevych, Iryna",
    booktitle = "Proceedings of the 2019 Conference on Empirical Methods in Natural Language Processing",
    month = "11",
    year = "2019",
    publisher = "Association for Computational Linguistics",
    url = "https://arxiv.org/abs/1908.10084",
}
```

<!--
## Glossary

*Clearly define terms in order to be accessible across audiences.*
-->

<!--
## Model Card Authors

*Lists the people who create the model card, providing recognition and accountability for the detailed work that goes into its construction.*
-->

<!--
## Model Card Contact

*Provides a way for people who have updates to the Model Card, suggestions, or questions, to contact the Model Card authors.*
-->