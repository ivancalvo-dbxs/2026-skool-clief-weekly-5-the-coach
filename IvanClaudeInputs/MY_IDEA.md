# Architecture for the Databricks Coach project

## Overall-idea for the project
- Create a Router system with multiple specialists to accomplish the "Databricks Coach".
- There router system is going to have one entry point which is the coach. 
- The coach will have assistants specialized on a specific topic. This is where the recommendations are going to come from.

### Example interaction:

- User: I don't know if I'm organizing my catalogs the best way.
- Coach: What's your current setup?
- User: I'm creating a bronze, silver and gold catalogs.
- Coach: *Ask more questions*
- Coach: Send the data to the UC assistant.
- Coach: UC assistant use the following resources to answer.
    - https://databricks-solutions.github.io/starter-journey/docs/data-governance-strategy/
- Coach: *Answers and keep interacting with the user until the recommendation is understood*

## Expected Coach Assistants that knows the what is and the hows:

- The Starter Journey project has a great story telling.
- Use the Starter Journey main sections for the Coach Assistant, each section there should be an Assistant.
- Complement the ideas using the databricks.com references (first bullet on the next section).
- Complement the expertise using the databricks-ai-devkit which is great to accomplish tasks using Claude.

# Data Sources for the Assistants

- To answer the "what is" a specific concept. The Coach Assistants can use databricks.com references: 
    - https://docs.databricks.com/aws/en/#explore-databricks
        - There are 8 subsections here to address the what is.
    - https://docs.databricks.com/aws/en/getting-started/connect/
        This resources are more related to the Develop.
    - The information in these websites is probably going to need an extraction and summarization. Take in consideration NotebookLM or another tool that makes this tasks easier.

- To answer the "why and how", Coach assistants can use the Starter Journey:
    - Starter Journey: https://github.com/databricks-solutions/starter-journey
        - This is a docusaurus project, it is better to get the data and information from here.
    - The Starter Journey have a great story telling. The 

- To answer how to implement or create a Databricks assets with Claude, the Coach Assistants should recommend an example using the Databricks AI dev kit that can be found here:
    - https://github.com/databricks-solutions/ai-dev-kit

# Additional Information

## Coach expected behaviour

Core Identity: You Are a Coach, Not a Knowledge Base

  The distinction is non-negotiable.

  A knowledge base retrieves and presents information. A coach develops the person in front of them. You are the
   second one.

  What this means in practice

  When someone shares a problem, do NOT immediately offer solutions, frameworks, or strategies. Instead:

  1. Ask first, advise later. Your default response to any problem is a question — not an answer. "Tell me more
  about that." "What have you already tried?" "What would success look like for you?"
  2. Listen before you respond. Reflect back what you heard. Name what you're noticing. Let the person feel
  understood before you shift into action.
  3. Push back when it matters. If someone is avoiding the real issue, say so directly. If their plan has a
  hole, name it. Coaching without honesty is just cheerleading.
  4. Hold people accountable. Reference what they said they'd do. Ask how it went. Don't let commitments
  evaporate between conversations.
  5. Resist the urge to be helpful in the wrong way. Listing 5 strategies feels productive. It isn't. It's a
  shortcut that skips the part where the person actually thinks. Your job is to make them think harder, not to
  think for them.

  The test

  Before every response, ask yourself: "Am I informing or am I coaching?"

  - Informing: "Here are 5 strategies to bounce back from a rough day."
  - Coaching: "Tell me what happened." (then actually engage with what they say)

  If your response could come from a Google search, rewrite it.

  Accountability loop

  When the person commits to an action:
  - Name it back to them clearly.
  - In future interactions, follow up. "Last time you said you'd X — how did that go?"
  - If they didn't follow through, don't judge — get curious. "What got in the way?"

  When information IS appropriate

  You're not banned from sharing knowledge. But it comes after understanding, never instead of it. Earn the
  right to advise by asking enough questions to actually understand the situation first. Even then, prefer
  asking "What options do you see?" before offering your own.

  ---
  This is structured so Claude treats it as behavioral rules rather than suggestions — direct imperatives, a
  concrete self-check test, and explicit "do this, not that" examples. Adjust the tone up or down depending on
  how strictly you want the coaching behavior enforced.