# main_single_file.py

import os
from crewai import Agent, Task, Crew, Process
from langchain_openai import ChatOpenAI

# --- My API Key Configuration ---
# I need to set OpenAI API key as an environment variable. 
# It's the most secure way! For example, on Linux/macOS:
# export OPENAI_API_KEY="your_openai_api_key_here"
# On Windows (in Command Prompt):
# set OPENAI_API_KEY="your_openai_api_key_here"
#
# If you're just testing and absolutely need to, you could uncomment the line below 
# and replace "OPENAI_API_KEY" with your key. But I don't recommend it for anything serious!
# os.environ["OPENAI_API_KEY"] = "OPENAI_API_KEY"

# Now, I'll initialize the language model (LLM) my agents will use.
llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0.7)

# --- My Agent Definitions ---

class MyContentAgents:
    def __init__(self):
        self.llm = llm # My agents will use the LLM I just set up!

    def planner_agent(self):
        return Agent(
            role='Content Planner',
            goal='Create a detailed, SEO-optimized content outline for an article',
            backstory="""I am an expert content strategist with a deep understanding of SEO, audience engagement, and content structuring. 
                         I excel at breaking down complex topics into clear, concise, and actionable outlines that guide writers to produce high-quality,
                         fact-checked content. My focus is on maximizing readability and search engine visibility.""",
            verbose=True,
            allow_delegation=False,
            llm=self.llm
        )

    def writer_agent(self):
        return Agent(
            role='Article Writer',
            goal='Draft a comprehensive, engaging, and fact-checked article based on a provided outline',
            backstory="""I am a seasoned article writer known for my ability to craft compelling narratives and integrate complex information seamlessly.
                         I am meticulous about factual accuracy, conduct thorough research, and can adapt my writing style to various tones and audiences.
                         I always keep SEO best practices in mind while writing.""",
            verbose=True,
            allow_delegation=False,
            llm=self.llm
        )

    def editor_agent(self):
        return Agent(
            role='Content Editor',
            goal='Review, refine, and optimize articles for clarity, accuracy, and SEO compliance',
            backstory="""I am a highly experienced and detail-oriented editor with an eagle eye for grammar, syntax, and factual errors.
                         I am also an SEO expert who can fine-tune content to improve its search engine ranking.
                         My ultimate goal is to ensure the article is polished, professional, and adheres to all editorial guidelines.""",
            verbose=True,
            allow_delegation=False,
            llm=self.llm
        )

# --- My Task Definitions ---

class MyContentTasks:
    def plan_article_outline(self, agent, topic, requirements):
        return Task(
            description=f"""
            Based on the following topic and requirements, create a detailed content outline for an article.
            
            Topic: {topic}
            Requirements: {requirements}
            
            The outline should include:
            - A compelling title.
            - Main headings and subheadings.
            - Key points to cover under each section.
            - Suggested SEO keywords to naturally integrate.
            - Target audience consideration.
            - Desired tone and style.
            
            Ensure the outline is structured logically for readability and SEO.
            """,
            agent=agent,
            expected_output="A comprehensive, SEO-optimized article outline in markdown format."
        )

    def write_article(self, agent, topic, outline):
        return Task(
            description=f"""
            Write a full, comprehensive article based on the following topic and detailed outline.
            
            Topic: {topic}
            Outline: {outline}
            
            Key responsibilities:
            - Expand on each point in the outline, providing detailed explanations and examples.
            - Integrate the suggested SEO keywords naturally.
            - Maintain a consistent tone and style as indicated in the outline.
            - Prioritize factual accuracy and indicate where specific facts or statistics might need verification.
            - Structure the article with proper markdown formatting (headings, paragraphs, bullet points).
            - The article should be engaging and informative for the target audience.
            """,
            agent=agent,
            expected_output="A well-written, comprehensive article in markdown format, ready for editing."
        )

    def edit_and_optimize_article(self, agent, article, requirements):
        return Task(
            description=f"""
            Review and refine the following article for clarity, factual accuracy, grammar, spelling, punctuation, and overall coherence.
            
            Article to edit: {article}
            Original Requirements for context: {requirements}
            
            Key responsibilities:
            - Proofread meticulously for any errors.
            - Verify factual accuracy where indicated or if anything seems questionable.
            - Improve sentence structure, flow, and readability.
            - Enhance SEO by ensuring proper keyword density and suggesting meta descriptions/title tags if not present.
            - Ensure the article adheres strictly to the original requirements and editorial guidelines (e.g., tone, style).
            - Provide a final, polished version of the article.
            """,
            agent=agent,
            expected_output="A final, polished, fact-checked, and SEO-optimized article in markdown format."
        )

# --- My Main Execution Logic ---

def main():
    print("## Welcome to My Automated Content Creator Crew!")
    print('--------------------------------------------------')

    topic = input("Enter the topic for the article: ")
    requirements = input("Enter any specific requirements or editorial guidelines (e.g., target audience, tone, keywords): ")

    # Now, I'll initialize my agents and tasks.
    my_agents = MyContentAgents()
    my_tasks = MyContentTasks()

    planner = my_agents.planner_agent()
    writer = my_agents.writer_agent()
    editor = my_agents.editor_agent()

    # I'll define the tasks for my crew.
    plan_task = my_tasks.plan_article_outline(
        agent=planner,
        topic=topic,
        requirements=requirements
    )

    write_task = my_tasks.write_article(
        agent=writer,
        topic=topic,
        outline=plan_task # The output from my planner feeds directly into my writer!
    )

    edit_task = my_tasks.edit_and_optimize_article(
        agent=editor,
        article=write_task, # The writer's draft goes straight to my editor!
        requirements=requirements
    )

    # Time to assemble the crew!
    my_content_crew = Crew(
        agents=[planner, writer, editor],
        tasks=[plan_task, write_task, edit_task],
        process=Process.sequential, # My agents work in a clear, defined order.
        verbose=2 # I like to see what my agents are thinking!
    )

    print("\n## Initiating My Content Creation Process...\n")
    result = my_content_crew.kickoff()

    print("\n-------------------------------------------------")
    print("## Content Creation Finished!")
    print("### Here's the Final Article:")
    print(result)
    print("-------------------------------------------------")

if __name__ == "__main__":
    main()
