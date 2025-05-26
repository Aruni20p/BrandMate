# BrandMate
# Technical Diagrammatic Explanation of LangGraph in BrandMate

# Abstract
This document provides a technical diagrammatic explanation of the BrandMate workflow implemented using LangGraph. The diagram illustrates the flow of the BrandMateState
through a directed acyclic graph (DAG), with nodes representing agent functions and
edges defining execution order. Key LangGraph operations such as add_node, add_edge,
set_entry_point, compile, and invoke are highlighted to show how the workflow is constructed and executed.

# 1 Overview
The BrandMate workflow uses LangGraph to orchestrate a sequence of agents that transform a
user input into a LinkedIn post with engagement metrics and reinforcement learning feedback.
The BrandMateState dictionary serves as a shared state, passed through each node (agent
function) in a directed acyclic graph (DAG). LangGraph’s core methods (add_node, add_edge,
set_entry_point, compile, and invoke) define and execute the workflow.

# 2 State Evolution
The BrandMateState evolves as it passes through each node. The table below lists the keys
added or updated by each agent:
Table 1: BrandMateState Keys by Node
Node (Agent) State Keys Updated
user_interaction_agent tone, target, goal
brand_identity_agent style_guide
content_strategist_agent topic
seo_agent seo_package
content_generator_agent post
post_editor_agent post
publishing_agent metrics
rl_feedback_agent q_table, feedback

# 3 Workflow Diagram
The diagram below illustrates the LangGraph workflow, showing the sequence of nodes and
the state updates they perform. Each node corresponds to an agent function registered with
workflow.add_node, and edges are defined using workflow.add_edge.

# LangGraph Technical Architecture for BrandMate
![image](https://github.com/user-attachments/assets/3a0f5015-088b-48be-a022-f0da805253e4)


# 4 LangGraph Implementation

The workflow is constructed using LangGraph’s StateGraph class. Below is the code that sets
up the graph, with annotations explaining each operation:
1 from langgraph . graph import StateGraph , END
2
3 # Initialize the StateGraph with BrandMateState
4 workflow = StateGraph ( BrandMateState )
5
6 # Register nodes ( agent functions ) using add_node
7 workflow . add_node (" user_interaction ", user_interaction_agent ) % Maps
input to state
8 workflow . add_node (" brand_identity ", brand_identity_agent ) %
Defines style guide
9 workflow . add_node (" content_strategist ", content_strategist_agent ) %
Selects topic
10 workflow . add_node (" seo ", seo_agent ) %
Fetches SEO data
11 workflow . add_node (" content_generator ", content_generator_agent ) %
Generates post
12 workflow . add_node (" post_editor ", post_editor_agent ) %
Refines post
13 workflow . add_node (" publishing ", publishing_agent ) %
Simulates posting
14 workflow . add_node (" rl_feedback ", rl_feedback_agent ) %
Computes Q - learning feedback
15
16 # Define edges to specify execution order
17 workflow . add_edge (" user_interaction ", " brand_identity ")
18 workflow . add_edge (" brand_identity ", " content_strategist ")
19 workflow . add_edge (" content_strategist ", " seo ")
20 workflow . add_edge (" seo ", " content_generator ")
21 workflow . add_edge (" content_generator ", " post_editor ")
22 workflow . add_edge (" post_editor ", " publishing ")
23 workflow . add_edge (" publishing ", " rl_feedback ")
24 workflow . add_edge (" rl_feedback ", END )
25
26 # Set the entry point for execution
27 workflow . set_entry_point (" user_interaction ")
28
29 # Compile the graph into an executable app
30 app = workflow . compile ()
31
32 # Invoke the workflow with an initial state
33 initial_state = {" user_input ": " Create a high - engagement LinkedIn
post , emotional tone , for women startup founders , to grow
followers "}
34 result = app . invoke ( initial_state ) % Executes the graph , passing
state through nodes

# 5 Technical Workflow
The workflow operates as follows:
▷ Initialization: The StateGraph is initialized with BrandMateState, a typed dictionary
defining keys like user_input, tone, and post.
▷ Node Registration: Each agent function is registered using workflow.add_node(node_id,
function), mapping a unique identifier to its corresponding function.
▷ Edge Definition: Edges are added with workflow.add_edge(from_node, to_node) to
define the sequential flow from user_interaction to END.
▷ Entry Point: workflow.set_entry_point("user_interaction") specifies the starting
node.
▷ Compilation: workflow.compile() creates an executable app that manages state transitions.
▷ Execution: app.invoke(initial_state) runs the graph, passing the BrandMateState
through each node. Each agent reads and updates the state, which is propagated to the
next node until reaching END.
6 Example Execution
The workflow is invoked with an initial state, and the final output includes the generated post,
metrics, and feedback:
1 initial_state = {
2 " user_input ": " Create a high - engagement LinkedIn post , emotional
tone , for women startup founders , to grow followers "
3 }
4 result = app . invoke ( initial_state )
5 print (" Generated Post :", result [" post "])
6 print (" Metrics :", result [" metrics "])
7 print (" Feedback :", result [" feedback "])
8 print ("Q- table :", result [" q_table "])

# 7 Conclusion
The BrandMate workflow leverages LangGraph’s StateGraph to create a modular, stateful
pipeline. The diagram and code snippets illustrate how add_node, add_edge, set_entry_point,
compile, and invoke orchestrate the flow of BrandMateState through agent functions. This
architecture ensures a clear separation of concerns, scalability, and the ability to integrate reinforcement learning for optimization.
