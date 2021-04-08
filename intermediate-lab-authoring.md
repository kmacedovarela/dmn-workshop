# Vacation Days - Authoring Decisions

Now, let's create our decision model.

## Input Nodes

The problem statement describes a number of different inputs to our decision:

-   **Age** of the employee

-   **Years of Service** of the employee

Therefore, we should create two input nodes, one for each input:

1.  Add an **Input** node to the diagram by clicking on the **Input** node icon and placing it in the DRD. ![Input]({% image_path add-drd-input-node.png %})

2.  Double-click on the node to set the name. We will name this node `Age`.

3.  With the `Age` node selected, open the property panel. Set the **data type** to `number`.

    ![data type]({% image_path drd-input-node-propery-output-data-type.png %})

1.  In the same way, create an **Input** node for `Years of Service`. This node should also have its **data type** set to `number`.

    ![Nodes Complete]({% image_path drd-decision-nodes-complete.png %})

1.  Save the model.

## Constants

The problem statement describes that every employee receives at least 22 days. So, if no other decisions apply, an employee receives 22 days. This is can be seen as a constant input value into our decision model. In DMN we can model such constant inputs with a **Decision** node with a **Literal** boxed expression that defines the constant value:

1.  Add a **Decision** node to the DRD

    ![Decision Node]({% image_path add-drd-decision-node.png %})

1.  Give the node the name `Base Vacation Days`.

2.  Click on the node to select it and open the property panel. Set the node’s **data type** to `number`.

3.  Click on the node and click on the **Edit** icon to open the expression editor.

    ![Edit Decision Node]({% image_path drd-decision-node-edit.png %})

1.  In the expression editor, click on the box that says **Select expression** and select **Literal expression**.

    ![Select Expression]({% image_path select-expression.png %})

1.  Simply set the **Literal Expression** to `22`, the number of base vacation days defined in the problem statement.

    ![Select Expression]({% image_path base-vacation-days-literal-expression.png %})

1.  Save the model.

## Decisions

The problem statement defines 3 decisions which can cause extra days to be given to employees based on various criteria. Let’s simply call these decision:

-   Extra days case 1

-   Extra days case 2

-   Extra days case 3

Although these decisions could be implemented in a single decision node, we’ve decided, in order to improve maintainability of the solution, to define these decisions in 3 separate decision nodes.

1.  In your DRD, create 3 decision nodes with these given names. Set their **data types** to `number`.

2.  We need to attach both input nodes, **Age** and **Years of Service** to all 3 decision nodes. We can do this by clicking on an Input node, clicking on its arrow icon, and attaching the arrow to the Decision node.

    ![Select Expression]({% image_path add-drd-three-decision-nodes.png %})

1.  Select the **Extra days case 1** node and open its expression editor by clicking on the **Edit** button.

2.  Select the expression **Decision Table** to create a boxed expression implemented as a decision table.

    ![Select Expression]({% image_path drd-decision-node-expression.png %})

3.  The first case defines 2 decisions which can be modelled with 2 rows in our decision table as such:

    1.  employees younger than 18 or at least 60 years will receive 5 extra days, or …

    2.  employees with at least 30 years of service will receive 5 extra days

    ![Select Expression]({% image_path decision-table-case-1.png %})

1. To add new lines to your table, right click the first column and select "Insert below"

    ![Select Expression]({% image_path decision-table-new-1-new-line.png %})

1.  Note that the **hit-policy** of the decision table is by default set to `U`, which means `Unique`. This implies that only one rule is expected to fire for a given input. In this case however, we would like to set it to `Collect Max`, as, for a given input, multiple decisions might match, but we would like to collect the output from the rule with the highest number of additional vacation days. To do this, click on the `U` in the upper-left corner of the decision table. Now, set the **Hit Policy** to `Collect` and the **Builtin Aggregator** to `MAX`.

    ![Decision Table Hit Policy]({% image_path decision-table-hit-policy.png %}) 

1. Finally, we need to set the default result of the decision. This is the result that will be returned when none of the rules match the given input. This is done as follows: .. Select the output/result column of the decision table. In this case this is the column `Extra days case 1` .. Open the properties panel on the right-side of the editor. .. Expand the **Default output** section. .. Set the `Default output property` to `0`. 

    ![Decision Table Default Output]({% image_path decision-table-default-output.png %}) 

1. Save the model

1. The other two decisions can be implemented in the same way. Now, implement the following two decision tables:

    * Case 2:
 
        ![Decision Table Case 2]({% image_path decision-table-case-2.png %})
    
    * Case 3:
  
        ![Decision Table Case 3]({% image_path decision-table-case-3.png %})


## Total Vacation Days

The total vacation days needs to be determined from the base vacation days and the decisions taken by our 3 decision nodes. As such, we need to create a new Decision node, which takes the output of our 4 Decision nodes (3 decision tables and a literal expression) as input and determines the final output. To do this, we need to:

1.  Create a new Decision node in the model. Give the node the name `Total Vacation Days` and set its **data type** to `number`.

2.  Connect the 4 existing Decision nodes to the node. This defines that the output of these nodes will be the input of the next node.

    ![DRD Complete]({% image_path drd-complete.png %})

1.  Click on the `Total Vacation Days` node and click on **Edit** to open the expression editor. Configure the expression as a literal expression.

2.  We need to configure the following logic:

    1.  Everyone gets the Base Vacation Days.

    2.  If both case 1 and case 3 add extra days, only the extra days of one of this decision is added. So, in that case we take the maximum.

    3.  If case 2 adds extra days, add them to the total.

3.  The above logic can be implemented with the following FEEL expression:

    ![Total Vacation Days Expression]({% image_path total-vacation-days-expression.png %})

1.  Save the completed model.

Deploying the Decision Service
==============================

With our decision model completed, we can now package our DMN model in a Deployment Unit (KJAR) and deploy it on the Execution Server. To do this:

1.  In the bread-crumb navigation in the upper-left corner, click on `vacation-days-decisions` to go back to the project’s Library View.

2.  Click on the **Deploy** button in the upper-right corner of the screen. This will package our DMN mode in a Deployment Unit (KJAR) and deploy it onto the Execution Server (KIE-Server).

3.  Go to the **Execution Servers** perspective by clicking on "Menu → Deploy → Execution Servers". You will see the **Deployment Unit** deployed on the Execution Server.

