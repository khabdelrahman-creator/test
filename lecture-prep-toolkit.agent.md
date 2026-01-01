import React, { useState } from 'react';
import { BookOpen, CheckSquare, Lightbulb, FileText, ChevronDown, ChevronUp, Copy, Check } from 'lucide-react';

const LectureToolkit = () => {
  const [activeTab, setActiveTab] = useState('templates');
  const [expandedSections, setExpandedSections] = useState({});
  const [copiedStates, setCopiedStates] = useState({});

  const toggleSection = (section) => {
    setExpandedSections(prev => ({
      ...prev,
      [section]: !prev[section]
    }));
  };

  const copyToClipboard = (text, id) => {
    navigator.clipboard.writeText(text);
    setCopiedStates(prev => ({ ...prev, [id]: true }));
    setTimeout(() => {
      setCopiedStates(prev => ({ ...prev, [id]: false }));
    }, 2000);
  };

  const templates = [
    {
      id: 'research-synthesis',
      title: 'Research Synthesis & Literature Review',
      category: 'Research Phase',
      prompt: `You are an expert academic researcher in [SUBJECT FIELD]. I'm preparing a lecture on [SPECIFIC TOPIC].

Please help me synthesize recent research by:
1. Identifying 5-7 key concepts or findings from the last 3-5 years
2. Explaining how these concepts relate to each other
3. Highlighting any debates or controversies in the field
4. Suggesting 2-3 compelling real-world applications or examples
5. Noting any common student misconceptions about this topic

Context about my students: [e.g., "3rd year undergraduates with basic statistics background"]
Lecture duration: [e.g., "50 minutes"]

Format your response with clear headings for each section.`,
      tips: ['Replace bracketed placeholders with your specifics', 'Add student background for targeted explanations', 'Follow up with: "What primary sources should I read first?"']
    },
    {
      id: 'lecture-outline',
      title: 'Structured Lecture Outline',
      category: 'Content Structuring',
      prompt: `Create a detailed lecture outline for a [DURATION]-minute class on [TOPIC] for [STUDENT LEVEL].

Learning Objectives (use Bloom's Taxonomy):
- [Add 2-3 specific objectives, e.g., "Students will be able to analyze..."]

Structure the outline with:
1. Opening Hook (3-5 min) - engaging question or scenario
2. Core Content Sections (with time allocations)
   - For each section: Key points, examples, potential visuals
3. Active Learning Moments (2-3 throughout)
4. Common Misconceptions to Address
5. Summary & Connection to Next Topic (5 min)

Include:
- Suggested discussion questions
- Points where students might struggle
- Transitions between sections

Student context: [Prior knowledge, class size, learning environment]`,
      tips: ['Start with clear learning objectives', 'Request time allocations for pacing', 'Ask for variation: "Give me 3 different opening hooks"']
    },
    {
      id: 'explanation-generator',
      title: 'Multi-Level Explanations',
      category: 'Content Creation',
      prompt: `I need to explain [CONCEPT] to my students. Please provide THREE different explanations:

1. NOVICE LEVEL (ELI5 style)
   - Use everyday analogies
   - Avoid jargon
   - Focus on intuitive understanding
   - Include a relatable metaphor

2. INTERMEDIATE LEVEL
   - Introduce technical terminology with definitions
   - Show how concepts connect
   - Include a worked example
   - Address one common misconception

3. ADVANCED LEVEL
   - Use precise academic language
   - Discuss nuances and edge cases
   - Connect to related theories/research
   - Pose an extension question for deeper thinking

Context: [Subject area, specific student challenges you've observed]

After each explanation, suggest one follow-up question I could ask students to check understanding.`,
      tips: ['Use this for difficult concepts students struggle with', 'Generate multiple analogies to find the best fit', 'Combine levels based on your class composition']
    },
    {
      id: 'assessment-design',
      title: 'Assessment & Quiz Generation',
      category: 'Assessment',
      prompt: `Design assessment questions for [TOPIC] covering these learning objectives:
[Paste your 2-4 learning objectives]

Create a balanced set of 10 questions distributed across Bloom's Taxonomy:

REMEMBER (2 questions):
- Multiple choice or fill-in-blank
- Test basic recall of facts/definitions

UNDERSTAND (3 questions):
- Short answer or matching
- Require explanation or summarization

APPLY (2 questions):
- Problem-solving or scenarios
- Use knowledge in new situations

ANALYZE (2 questions):
- Compare/contrast or identify patterns
- Break down information

EVALUATE (1 question):
- Critique or justify
- Make judgments based on criteria

For each question provide:
- The question text
- Correct answer with brief explanation
- Common wrong answers and why students choose them
- Estimated difficulty (easy/medium/hard)

Student level: [Year and background]`,
      tips: ['Specify open-ended vs. multiple choice preference', 'Request answer keys separately for security', 'Ask for question variations to prevent cheating']
    },
    {
      id: 'visual-design',
      title: 'Visual Aid & Diagram Planning',
      category: 'Visual Content',
      prompt: `I'm creating visual aids for a lecture on [TOPIC]. Help me plan effective visuals:

1. CONCEPT MAP/DIAGRAM
   Describe a visual representation showing:
   - Main concept in center
   - 4-6 related subconcepts
   - Relationships/connections between elements
   - Use clear labels

2. PROCESS FLOWCHART
   Outline steps for [SPECIFIC PROCESS]:
   - Sequential steps with decision points
   - Color coding suggestions (what should be emphasized)
   - Where to add icons or symbols

3. COMPARISON TABLE
   Create a structure comparing [OPTION A] vs [OPTION B]:
   - 5-6 comparison criteria (rows)
   - Key differentiators highlighted
   - Visual hierarchy suggestions

4. DATA VISUALIZATION
   For this data/concept: [DESCRIBE DATA]
   - Recommend: bar chart, line graph, pie chart, scatter plot?
   - What should X and Y axes show?
   - What story does this tell?

For each visual, explain its pedagogical purpose and when to introduce it in the lecture.`,
      tips: ['Use AI to plan visuals, then create in PowerPoint/Canva', 'Request accessibility considerations (color blindness, text size)', 'Ask: "What would confuse students about this visual?"']
    },
    {
      id: 'case-study',
      title: 'Interactive Case Study Development',
      category: 'Active Learning',
      prompt: `Create an interactive case study for teaching [CONCEPT/TOPIC] to [STUDENT LEVEL].

CASE SCENARIO (realistic and relatable):
- Set the scene with a real-world problem
- Include relevant details and context
- Present a dilemma or challenge requiring the concept
- Length: 250-350 words

TEACHING STRUCTURE:
1. Initial Questions (before revealing full case)
   - 2-3 activating prior knowledge

2. Case Analysis Questions
   - What's the core problem?
   - What concepts/theories apply?
   - What data/information is needed?

3. Small Group Discussion Prompts (3-4 students)
   - Open-ended questions
   - Multiple valid approaches

4. Possible Solutions
   - 2-3 viable approaches
   - Pros and cons of each
   - Which would you recommend and why?

5. Debrief Questions for Full Class
   - Connect back to learning objectives
   - Extend thinking beyond the case

Include:
- Time allocation for each phase
- Common student responses
- Key teaching points to emphasize

Domain context: [Industry, field, or setting relevant to your students]`,
      tips: ['Make cases relevant to student career goals', 'Include ambiguity - not one "right" answer', 'Request variations for different class sizes']
    }
  ];

  const rubric = {
    categories: [
      {
        name: 'Accuracy & Factual Correctness',
        criteria: [
          { level: 'Excellent (4)', description: 'All facts, dates, formulas, and concepts are verifiable and correct. Claims are supported by current research/sources.' },
          { level: 'Good (3)', description: 'Minor factual errors that do not affect core understanding. Most information is accurate and current.' },
          { level: 'Acceptable (2)', description: 'Some significant errors or outdated information. Requires moderate fact-checking and correction.' },
          { level: 'Unacceptable (1)', description: 'Multiple factual errors, hallucinations, or misleading information. Cannot be used without major revision.' }
        ],
        checkpoints: ['Verify dates, statistics, and formulas', 'Cross-reference claims with authoritative sources', 'Check for anachronisms or outdated theories']
      },
      {
        name: 'Pedagogical Appropriateness',
        criteria: [
          { level: 'Excellent (4)', description: 'Perfectly matched to student level. Clear learning progression. Examples are relevant and aid understanding.' },
          { level: 'Good (3)', description: 'Generally appropriate with minor adjustments needed. Good examples with room for improvement.' },
          { level: 'Acceptable (2)', description: 'Some content too advanced or too simple. Examples need significant adaptation.' },
          { level: 'Unacceptable (1)', description: 'Inappropriate complexity. Poor example selection. Does not match learning objectives.' }
        ],
        checkpoints: ['Does vocabulary match student level?', 'Are examples culturally relevant?', 'Does complexity align with prerequisites?']
      },
      {
        name: 'Clarity & Organization',
        criteria: [
          { level: 'Excellent (4)', description: 'Logical flow with clear transitions. Complex ideas broken down effectively. Jargon explained.' },
          { level: 'Good (3)', description: 'Generally clear with occasional unclear passages. Structure is logical overall.' },
          { level: 'Acceptable (2)', description: 'Some confusing sections. Organization needs restructuring. Key terms need clarification.' },
          { level: 'Unacceptable (1)', description: 'Confusing structure. Unclear explanations. Technical terms used without definition.' }
        ],
        checkpoints: ['Can students follow the logic?', 'Are transitions smooth?', 'Is technical vocabulary explained?']
      },
      {
        name: 'Originality & Authenticity',
        criteria: [
          { level: 'Excellent (4)', description: 'Fresh perspectives and unique examples. Allows for instructor voice. Avoids generic phrasing.' },
          { level: 'Good (3)', description: 'Some original elements. Can be personalized with moderate effort.' },
          { level: 'Acceptable (2)', description: 'Mostly generic content. Sounds AI-generated. Requires significant personalization.' },
          { level: 'Unacceptable (1)', description: 'Entirely generic. No distinct voice. Cannot be distinguished from other AI outputs.' }
        ],
        checkpoints: ['Add personal anecdotes', 'Replace generic examples with field-specific ones', 'Inject your pedagogical style']
      },
      {
        name: 'Bias & Inclusivity',
        criteria: [
          { level: 'Excellent (4)', description: 'Inclusive language and diverse examples. No stereotypes. Accessible to all learners.' },
          { level: 'Good (3)', description: 'Generally inclusive with minor oversights. Mostly diverse representation.' },
          { level: 'Acceptable (2)', description: 'Some biased language or limited representation. Needs revision for inclusivity.' },
          { level: 'Unacceptable (1)', description: 'Contains stereotypes, biased assumptions, or exclusionary language.' }
        ],
        checkpoints: ['Are examples diverse in gender, culture, ability?', 'Is language accessible to non-native speakers?', 'Are there unconscious biases?']
      }
    ]
  };

  const exercises = [
    {
      id: 'ex1',
      title: 'Exercise 1: Transform Textbook Content',
      difficulty: 'Beginner',
      duration: '20 minutes',
      objective: 'Convert dense textbook material into an engaging lecture segment',
      scenario: 'You have this textbook passage on "The Krebs Cycle" (or use your own subject material):',
      sampleText: 'The citric acid cycle, also known as the Krebs cycle, is a series of chemical reactions used by aerobic organisms to release stored energy through the oxidation of acetyl-CoA derived from carbohydrates, fats, and proteins into carbon dioxide and chemical energy in the form of ATP.',
      tasks: [
        'Use AI to generate a conversational explanation suitable for 2nd-year biology students',
        'Request an everyday analogy to make the concept memorable',
        'Ask for 3 visualization suggestions',
        'Generate 2 formative assessment questions (one easy, one challenging)'
      ],
      samplePrompt: `Take this textbook definition and transform it into an engaging 5-minute lecture segment for 2nd-year biology students:

[PASTE TEXTBOOK TEXT]

Include:
1. A conversational explanation (avoid reading definitions)
2. An everyday analogy that makes the concept memorable
3. Three visualization suggestions I could use on slides
4. Two formative assessment questions:
   - One easy question to check basic understanding
   - One challenging question requiring application

Keep the scientific accuracy but make it engaging and accessible.`,
      sampleSolution: `SAMPLE AI OUTPUT:

1. CONVERSATIONAL EXPLANATION:
"Think of the Krebs cycle as your cell's central energy processing plant. You know how you eat food and somehow that becomes energy you can use? This is a key part of that magic. Your cells take the broken-down pieces of whatever you ate - whether it was a burger, salad, or pasta - and run them through this cycle to extract energy. It's like a recycling center that squeezes every bit of useful energy out of food molecules."

2. EVERYDAY ANALOGY:
"Imagine a car manufacturing assembly line that never stops. Raw materials enter, they get passed from station to station, and at each station something useful is extracted. The Krebs cycle is similar - acetyl-CoA enters, gets passed through 8 different enzyme 'stations,' and at each stop the cell extracts a bit of energy (ATP) and electron carriers. The cycle loops continuously as long as you have fuel coming in."

3. VISUALIZATION SUGGESTIONS:
- Circular diagram showing 8 steps with enzyme names, color-coded by what's gained at each step
- Animation showing acetyl-CoA entering and products (ATP, NADH, FADH2) leaving
- Side-by-side comparison: food on plate → Krebs cycle → ATP molecules (showing energy transformation)

4. FORMATIVE ASSESSMENT:
EASY: "What are the main inputs and outputs of the Krebs cycle?" (Tests basic recall)
CHALLENGING: "A patient has a mutation affecting the enzyme succinyl-CoA synthetase. How would this affect ATP production, and what symptoms might appear?" (Requires application and analysis)

EVALUATION:
✓ Accuracy: Scientifically correct, appropriate detail level
✓ Clarity: Jargon explained, logical flow
✓ Engagement: Analogy is relatable, explanation conversational
Note: Add your own clinical or research example to personalize`
    },
    {
      id: 'ex2',
      title: 'Exercise 2: Differentiated Explanations',
      difficulty: 'Intermediate',
      duration: '25 minutes',
      objective: 'Generate multiple versions of an explanation for diverse learner needs',
      scenario: 'Your class has diverse backgrounds. Some students grasp concepts quickly while others need more scaffolding.',
      concept: 'Standard Deviation (or choose your own concept)',
      tasks: [
        'Generate three versions: visual learner, verbal learner, and mathematical learner',
        'Create a version for students with math anxiety',
        'Develop an advanced extension for accelerated students',
        'Compare outputs from two different AI tools (Claude vs ChatGPT)'
      ],
      samplePrompt: `I need to explain "standard deviation" to a diverse statistics class. Create FOUR different explanations optimized for different learning needs:

1. VISUAL LEARNER VERSION
   - Describe what I should draw/show
   - Use spatial reasoning
   - Minimize equations

2. VERBAL/CONCEPTUAL LEARNER VERSION
   - Use analogies and stories
   - Explain the "why" before the "how"
   - Connect to everyday experiences

3. MATHEMATICAL LEARNER VERSION
   - Lead with the formula
   - Show step-by-step calculation
   - Emphasize logical progression

4. MATH-ANXIOUS STUDENT VERSION
   - Remove intimidating symbols initially
   - Build confidence with simple examples
   - Emphasize intuition over calculation

For each version, include:
- Main explanation (100-150 words)
- One concrete example
- A check-for-understanding question

Then suggest: How could I use all four in one 50-minute class?`,
      sampleSolution: `SAMPLE INTEGRATION STRATEGY:

TIMING:
- Minutes 0-10: Start with Version 2 (Verbal/Conceptual) to build intuition for everyone
- Minutes 10-20: Introduce Version 1 (Visual) - show bell curves, spread examples
- Minutes 20-30: Present Version 3 (Mathematical) - walk through calculation
- Minutes 30-40: Small group practice with Version 4 (Math-Anxious) provided as handout for those who need it
- Minutes 40-50: Advanced students work on extension problems while you support struggling students

PEDAGOGICAL NOTE:
The multi-explanation approach serves all students. Visual learners get their preferred mode, but ALSO benefit from hearing verbal explanations. Mathematical learners appreciate the formula, but the analogy helps them teach others. Math-anxious students build confidence through the scaffolded approach.

INSTRUCTOR REFLECTION:
✓ Did you generate truly different explanations or just reworded?
✓ Which version would you naturally teach? (Reveals your style)
✓ How would you assess which students need which version?`
    },
    {
      id: 'ex3',
      title: 'Exercise 3: Assessment Quality Control',
      difficulty: 'Advanced',
      duration: '30 minutes',
      objective: 'Critically evaluate and improve AI-generated assessment items',
      scenario: 'AI generated quiz questions for your lecture. Your job: identify flaws and improve them.',
      tasks: [
        'Generate 5 questions on your topic using AI',
        'Evaluate each using the rubric criteria',
        'Identify specific flaws (cognitive level, ambiguity, bias)',
        'Rewrite questions to improve quality',
        'Create an answer key with explanations'
      ],
      samplePrompt: `Generate 5 assessment questions about [YOUR TOPIC] for [STUDENT LEVEL]. Use this structure:

- 2 questions at "Apply" level (Bloom's)
- 2 questions at "Analyze" level
- 1 question at "Evaluate" level

After generating, I'll critique them. For now, just create the questions with:
- Question text
- Question type (MC, short answer, problem)
- Correct answer
- Why this question is valuable

Topic context: [Add relevant details]`,
      evaluation: `CRITICAL EVALUATION CHECKLIST:

COGNITIVE LEVEL
- Does it truly assess the claimed Bloom's level?
- Can students answer correctly without real understanding?
  
CLARITY
- Is the question unambiguous?
- Would two experts interpret it the same way?
  
FAIRNESS
- Can students with different backgrounds answer it?
- Does it require specific cultural knowledge?
  
AUTHENTICITY
- Does it assess something worth knowing?
- Would experts in the field value this knowledge?
  
DIFFICULTY CALIBRATION
- Too easy? Too hard? Just right?
- What percentage of prepared students should get this correct?

COMMON AI QUESTION FLAWS:
1. "All of the above" in multiple choice (lazy option)
2. Questions answerable with test-taking strategies alone
3. Overly academic language not matching student level
4. Questions with multiple defensible answers
5. Recall questions disguised as higher-order thinking`,
      flawedQuestions: [
        {
          q: 'What is photosynthesis?',
          flaw: 'Too broad, tests recall not understanding',
          improved: 'Explain why photosynthesis produces oxygen as a byproduct rather than retaining it.'
        },
        {
          q: 'The mitochondria is the powerhouse of the cell. True or False?',
          flaw: 'Rewards memorization of a phrase without understanding',
          improved: 'Which statement best explains why mitochondria are called the "powerhouse" of the cell? [with thoughtful options]'
        }
      ]
    }
  ];

  const toolComparison = [
    {
      tool: 'Claude (Anthropic)',
      strengths: ['Long-form content', 'Nuanced explanations', 'Following complex instructions', 'Artifact feature for interactive content', 'Strong at academic writing'],
      weaknesses: ['No real-time web browsing (in basic mode)', 'Can be verbose'],
      bestFor: ['Detailed lecture outlines', 'Multi-level explanations', 'Case study development', 'Creating rubrics and frameworks'],
      pricing: 'Free tier available; Pro $20/month',
      academicUse: 'Excellent for in-depth content development and pedagogical planning'
    },
    {
      tool: 'ChatGPT (OpenAI)',
      strengths: ['Wide adoption', 'GPT-4 with web browsing', 'Plugin ecosystem', 'Code generation', 'DALL-E integration'],
      weaknesses: ['Can be less detailed than Claude', 'Occasional consistency issues'],
      bestFor: ['Quick question generation', 'Visual content planning', 'Research summaries', 'Code examples for CS courses'],
      pricing: 'Free tier (GPT-3.5); Plus $20/month (GPT-4)',
      academicUse: 'Very versatile; excellent all-rounder for most teaching tasks'
    },
    {
      tool: 'Perplexity AI',
      strengths: ['Real-time web search', 'Source citations', 'Academic mode', 'Up-to-date information', 'Research-focused'],
      weaknesses: ['Less creative than Claude/ChatGPT', 'Better for facts than explanations'],
      bestFor: ['Finding recent research', 'Current event integration', 'Fact-checking', 'Literature reviews'],
      pricing: 'Free tier; Pro $20/month',
      academicUse: 'Best for research phase and staying current with field developments'
    },
    {
      tool: 'Google Gemini',
      strengths: ['Integrated with Google Workspace', 'Multimodal (text, image, video)', 'Long context window', 'Free access to advanced model'],
      weaknesses: ['Less specialized for education', 'Inconsistent quality'],
      bestFor: ['Analyzing academic papers', 'Multimodal content', 'Integration with Google Docs/Slides', 'YouTube video summaries'],
      pricing: 'Free; Gemini Advanced $20/month',
      academicUse: 'Good for faculty already in Google ecosystem'
    },
    {
      tool: 'Microsoft Copilot',
      strengths: ['Office 365 integration', 'PowerPoint generation', 'Teams integration', 'Enterprise security'],
      weaknesses: ['Requires Microsoft 365', 'Can be expensive', 'Less flexible for custom prompts'],
      bestFor: ['Creating PowerPoint slides', 'Formatting documents', 'Email drafting', 'Institutional adoption'],
      pricing: 'Included with Microsoft 365 Copilot subscription (~$30/user/month)',
      academicUse: 'Best for institutions with existing Microsoft infrastructure'
    }
  ];

  const TabButton = ({ id, label, icon: Icon }) => (
    <button
      onClick={() => setActiveTab(id)}
      className={`flex items-center gap-2 px-6 py-3 font-medium transition-all ${
        activeTab === id
          ? 'text-blue-600 border-b-2 border-blue-600 bg-blue-50'
          : 'text-gray-600 hover:text-gray-900 hover:bg-gray-50'
      }`}
    >
      <Icon size={20} />
      {label}
    </button>
  );

  return (
    <div className="min-h-screen bg-gray-50">
      <div className="bg-gradient-to-r from-blue-600 to-indigo-700 text-white p-8">
        <h1 className="text-4xl font-bold mb-2">AI-Powered Lecture Preparation Toolkit</h1>
        <p className="text-blue-100 text-lg">Complete resource guide for university professors</p>
      </div>

      <div className="border-b border-gray-200 bg-white sticky top-0 z-10 shadow-sm">
        <div className="max-w-7xl mx-auto flex overflow-x-auto">
          <TabButton id="templates" label="Prompt Templates" icon={FileText} />
          <TabButton id="rubric" label="Evaluation Rubric" icon={CheckSquare} />
          <TabButton id="exercises" label="Practice Exercises" icon={Lightbulb} />
          <TabButton id="tools" label="Tool Comparison" icon={BookOpen} />
        </div>
      </div>

      <div className="max-w-7xl mx-auto p-8">
        {activeTab === 'templates' && (
          <div className="space-y-6">
            <div className="bg-blue-50 border border-blue-200 rounded-lg p-6">
              <h2 className="text-xl font-bold text-blue-900 mb-2">How to Use These Templates</h2>
              <ul className="text-blue-800 space-y-1">
                <li>• Replace [BRACKETED] sections with your specific information</li>
                <li>• Start with the basic template, then refine based on results</li>
                <li>• Iterate: If the output is not quite right, add more context and try again</li>
                <li>• Copy the template and paste directly into Claude, ChatGPT, or your preferred AI</li>
              </ul>
            </div>

            {templates.map((template) => (
              <div key={template.id} className="bg-white rounded-lg shadow-md overflow-hidden border border-gray-200">
                <div
                  className="bg-gradient-to-r from-indigo-500 to-purple-600 text-white p-4 cursor-pointer flex justify-between items-center"
                  onClick={() => toggleSection(template.id)}
                >
                  <div>
                    <h3 className="text-xl font-bold">{template.title}</h3>
                    <p className="text-indigo-100 text-sm">{template.category}</p>
                  </div>
                  {expandedSections[template.id] ? <ChevronUp /> : <ChevronDown />}
                </div>

                {expandedSections[template.id] && (
                  <div className="p-6">
                    <div className="bg-gray-50 rounded-lg p-4 mb-4 font-mono text-sm whitespace-pre-wrap relative">
                      {template.prompt}
                      <button
                        onClick={() => copyToClipboard(template.prompt, template.id)}
                        className="absolute top-2 right-2 p-2 bg-white rounded shadow hover:bg-gray-100 transition-colors"
                        title="Copy to clipboard"
                      >
                        {copiedStates[template.id] ? (
                          <Check size={16} className="text-green-600" />
                        ) : (
                          <Copy size={16} className="text-gray-600" />
                        )}
                      </button>
                    </div>

                    <div className="bg-yellow-50 border-l-4 border-yellow-400 p-4">
                      <h4 className="font-bold text-yellow-900 mb-2">Pro Tips:</h4>
                      <ul className="text-yellow-800 space-y-1">
                        {template.tips.map((tip, idx) => (
                          <li key={idx}>• {tip}</li>
                        ))}
                      </ul>
                    </div>
                  </div>
                )}
              </div>
            ))}
          </div>
        )}

        {activeTab === 'rubric' && (
          <div className="space-y-6">
            <div className="bg-white rounded-lg shadow-md p-6 border border-gray-200">
              <h2 className="text-2xl font-bold text-gray-900 mb-4">
                Evaluation Rubric for AI-Generated Educational Content
              </h2>
              <p className="text-gray-700 mb-4">
                Use this rubric to systematically evaluate AI outputs before using them in your teaching. 
                Score each category from 1-4, then calculate total score out of 20.
              </p>
              <div className="bg-gradient-to-r from-green-50 to-blue-50 border border-blue-200 rounded p-4 mb-6">
                <p className="font-semibold text-gray-900">Scoring Guide:</p>
                <p className="text-gray-700">18-20 = Ready to use | 15-17 = Minor edits needed | 12-14 = Significant revision required | Below 12 = Start over with better prompt</p>
              </div>
            </div>

            {rubric.categories.map((category, idx) => (
              <div key={idx} className="bg-white rounded-lg shadow-md border border-gray-200 overflow-hidden">
                <div className="bg-gradient-to-r from-blue-500 to-indigo-600 text-white p-4">
                  <h3 className="text-xl font-bold">{category.name}</h3>
                </div>
                <div className="p-6">
                  <div className="space-y-3 mb-4">
                    {category.criteria.map((criterion, cIdx) => (
                      <div key={cIdx} className="border-l-4 border-blue-400 pl-4 py-2">
                        <p className="font-semibold text-gray-900">{criterion.level}</p>
                        <p className="text-gray-700">{criterion.description}</p>
                      </div>
                    ))}
                  </div>
                  <div className="bg-purple-50 border border-purple-200 rounded p-4">
                    <h4 className="font-bold text-purple-900 mb-2">Quality Checkpoints:</h4>
                    <ul className="text-purple-800 space-y-1">
                      {category.checkpoints.map((checkpoint, cpIdx) => (
                        <li key={cpIdx}>□ {checkpoint}</li>
                      ))}
                    </ul>
                  </div>
                </div>
              </div>
            ))}

            <div className="bg-orange-50 border border-orange-300 rounded-lg p-6">
              <h3 className="text-xl font-bold text-orange-900 mb-3">Red Flags - Reject Immediately If:</h3>
              <ul className="text-orange-800 space-y-2">
                <li>• Contains factual errors about core concepts in your field</li>
                <li>• Uses stereotypes or biased language</li>
                <li>• Includes unverifiable statistics or facts</li>
                <li>• Sounds generic and could apply to any course</li>
                <li>• Has pedagogical approaches that conflict with your teaching philosophy</li>
              </ul>
            </div>
          </div>
        )}

        {activeTab === 'exercises' && (
          <div className="space-y-6">
            <div className="bg-white rounded-lg shadow-md p-6 border border-gray-200">
              <h2 className="text-2xl font-bold text-gray-900 mb-4">
                Hands-On Practice Exercises
              </h2>
              <p className="text-gray-700">
                Complete these exercises to build practical AI integration skills. Each includes objectives, 
                sample prompts, and solutions with evaluation criteria.
              </p>
            </div>

            {exercises.map((exercise) => (
              <div key={exercise.id} className="bg-white rounded-lg shadow-md border border-gray-200 overflow-hidden">
                <div
                  className="bg-gradient-to-r from-green-500 to-teal-600 text-white p-4 cursor-pointer flex justify-between items-center"
                  onClick={() => toggleSection(exercise.id)}
                >
                  <div>
                    <h3 className="text-xl font-bold">{exercise.title}</h3>
                    <div className="flex gap-4 mt-1 text-sm">
                      <span className="bg-white bg-opacity-20 px-2 py-1 rounded">{exercise.duration}</span>
                      <span className="bg-white bg-opacity-20 px-2 py-1 rounded">{exercise.difficulty}</span>
                    </div>
                  </div>
                  {expandedSections[exercise.id] ? <ChevronUp /> : <ChevronDown />}
                </div>

                {expandedSections[exercise.id] && (
                  <div className="p-6 space-y-4">
                    <div className="bg-blue-50 border-l-4 border-blue-500 p-4">
                      <h4 className="font-bold text-blue-900 mb-2">Learning Objective:</h4>
                      <p className="text-blue-800">{exercise.objective}</p>
                    </div>

                    <div>
                      <h4 className="font-bold text-gray-900 mb-2">Scenario:</h4>
                      <p className="text-gray-700">{exercise.scenario}</p>
                      {exercise.sampleText && (
                        <div className="mt-2 bg-gray-100 p-3 rounded italic text-gray-700">
                          {exercise.sampleText}
                        </div>
                      )}
                      {exercise.concept && (
                        <p className="mt-2 font-semibold text-gray-800">Focus Concept: {exercise.concept}</p>
                      )}
                    </div>

                    <div>
                      <h4 className="font-bold text-gray-900 mb-2">Tasks:</h4>
                      <ol className="list-decimal list-inside space-y-1 text-gray-700">
                        {exercise.tasks.map((task, idx) => (
                          <li key={idx}>{task}</li>
                        ))}
                      </ol>
                    </div>

                    <div className="bg-purple-50 rounded-lg p-4">
                      <h4 className="font-bold text-purple-900 mb-2">Sample Prompt:</h4>
                      <div className="bg-white rounded p-3 font-mono text-sm whitespace-pre-wrap relative">
                        {exercise.samplePrompt}
                        <button
                          onClick={() => copyToClipboard(exercise.samplePrompt, `${exercise.id}-prompt`)}
                          className="absolute top-2 right-2 p-2 bg-gray-100 rounded shadow hover:bg-gray-200 transition-colors"
                        >
                          {copiedStates[`${exercise.id}-prompt`] ? (
                            <Check size={16} className="text-green-600" />
                          ) : (
                            <Copy size={16} className="text-gray-600" />
                          )}
                        </button>
                      </div>
                    </div>

                    {exercise.sampleSolution && (
                      <div className="bg-green-50 rounded-lg p-4">
                        <h4 className="font-bold text-green-900 mb-2">Sample Solution and Evaluation:</h4>
                        <div className="bg-white rounded p-3 text-sm whitespace-pre-wrap text-gray-700">
                          {exercise.sampleSolution}
                        </div>
                      </div>
                    )}

                    {exercise.evaluation && (
                      <div className="bg-yellow-50 rounded-lg p-4">
                        <h4 className="font-bold text-yellow-900 mb-2">Evaluation Framework:</h4>
                        <div className="bg-white rounded p-3 text-sm whitespace-pre-wrap text-gray-700">
                          {exercise.evaluation}
                        </div>
                      </div>
                    )}

                    {exercise.flawedQuestions && (
                      <div className="bg-red-50 rounded-lg p-4">
                        <h4 className="font-bold text-red-900 mb-3">Example Flawed Questions:</h4>
                        {exercise.flawedQuestions.map((fq, idx) => (
                          <div key={idx} className="bg-white rounded p-3 mb-3">
                            <p className="font-semibold text-gray-900 mb-1">Question: {fq.q}</p>
                            <p className="text-red-700 mb-1">Flaw: {fq.flaw}</p>
                            <p className="text-green-700">Improved: {fq.improved}</p>
                          </div>
                        ))}
                      </div>
                    )}
                  </div>
                )}
              </div>
            ))}

            <div className="bg-indigo-50 border border-indigo-300 rounded-lg p-6">
              <h3 className="text-xl font-bold text-indigo-900 mb-3">After Completing Exercises:</h3>
              <ul className="text-indigo-800 space-y-2">
                <li>• Reflect: Which prompting technique worked best for your discipline?</li>
                <li>• Document: Save your best prompts in a personal template library</li>
                <li>• Share: Discuss findings with colleagues in your department</li>
                <li>• Iterate: Try variations with different AI tools to compare outputs</li>
                <li>• Apply: Use these techniques in your actual course preparation</li>
              </ul>
            </div>
          </div>
        )}

        {activeTab === 'tools' && (
          <div className="space-y-6">
            <div className="bg-white rounded-lg shadow-md p-6 border border-gray-200">
              <h2 className="text-2xl font-bold text-gray-900 mb-4">
                AI Tool Comparison for Academic Use
              </h2>
              <p className="text-gray-700 mb-4">
                Each AI tool has unique strengths. This comparison helps you choose the right tool for specific tasks.
                Updated: January 2026
              </p>
              <div className="bg-green-50 border border-green-300 rounded p-4">
                <p className="font-semibold text-green-900">Pro Strategy:</p>
                <p className="text-green-800">Use multiple tools in your workflow. For example: Perplexity for research → Claude for detailed outlines → ChatGPT for quick question generation → Your expertise for final refinement.</p>
              </div>
            </div>

            {toolComparison.map((tool, idx) => (
              <div key={idx} className="bg-white rounded-lg shadow-md border border-gray-200 overflow-hidden">
                <div className="bg-gradient-to-r from-indigo-600 to-purple-700 text-white p-4">
                  <h3 className="text-2xl font-bold">{tool.tool}</h3>
                  <p className="text-indigo-100 mt-1">{tool.academicUse}</p>
                </div>
                <div className="p-6 space-y-4">
                  <div>
                    <h4 className="font-bold text-green-700 mb-2 flex items-center gap-2">
                      Strengths
                    </h4>
                    <ul className="space-y-1">
                      {tool.strengths.map((strength, sIdx) => (
                        <li key={sIdx} className="text-gray-700 pl-4">• {strength}</li>
                      ))}
                    </ul>
                  </div>

                  <div>
                    <h4 className="font-bold text-orange-700 mb-2 flex items-center gap-2">
                      Limitations
                    </h4>
                    <ul className="space-y-1">
                      {tool.weaknesses.map((weakness, wIdx) => (
                        <li key={wIdx} className="text-gray-700 pl-4">• {weakness}</li>
                      ))}
                    </ul>
                  </div>

                  <div className="bg-blue-50 rounded-lg p-4">
                    <h4 className="font-bold text-blue-900 mb-2">Best Use Cases:</h4>
                    <ul className="space-y-1">
                      {tool.bestFor.map((use, uIdx) => (
                        <li key={uIdx} className="text-blue-800">• {use}</li>
                      ))}
                    </ul>
                  </div>

                  <div className="flex justify-between items-center bg-gray-50 rounded p-3">
                    <span className="font-semibold text-gray-900">Pricing:</span>
                    <span className="text-gray-700">{tool.pricing}</span>
                  </div>
                </div>
              </div>
            ))}

            <div className="bg-white rounded-lg shadow-md p-6 border border-gray-200">
              <h3 className="text-xl font-bold text-gray-900 mb-4">Quick Reference Matrix</h3>
              <div className="overflow-x-auto">
                <table className="w-full text-sm">
                  <thead>
                    <tr className="bg-indigo-100">
                      <th className="p-3 text-left font-bold text-indigo-900">Task</th>
                      <th className="p-3 text-left font-bold text-indigo-900">Recommended Tool</th>
                      <th className="p-3 text-left font-bold text-indigo-900">Alternative</th>
                    </tr>
                  </thead>
                  <tbody className="divide-y divide-gray-200">
                    <tr className="hover:bg-gray-50">
                      <td className="p-3 font-medium">Research synthesis</td>
                      <td className="p-3 text-green-700">Perplexity AI</td>
                      <td className="p-3 text-gray-600">Claude + web search</td>
                    </tr>
                    <tr className="hover:bg-gray-50">
                      <td className="p-3 font-medium">Detailed lecture outline</td>
                      <td className="p-3 text-green-700">Claude</td>
                      <td className="p-3 text-gray-600">ChatGPT-4</td>
                    </tr>
                    <tr className="hover:bg-gray-50">
                      <td className="p-3 font-medium">Quick quiz questions</td>
                      <td className="p-3 text-green-700">ChatGPT</td>
                      <td className="p-3 text-gray-600">Claude</td>
                    </tr>
                    <tr className="hover:bg-gray-50">
                      <td className="p-3 font-medium">PowerPoint generation</td>
                      <td className="p-3 text-green-700">Microsoft Copilot</td>
                      <td className="p-3 text-gray-600">Claude (outline) + manual</td>
                    </tr>
                    <tr className="hover:bg-gray-50">
                      <td className="p-3 font-medium">Visual content ideas</td>
                      <td className="p-3 text-green-700">ChatGPT + DALL-E</td>
                      <td className="p-3 text-gray-600">Claude (descriptions)</td>
                    </tr>
                    <tr className="hover:bg-gray-50">
                      <td className="p-3 font-medium">Multi-level explanations</td>
                      <td className="p-3 text-green-700">Claude</td>
                      <td className="p-3 text-gray-600">ChatGPT-4</td>
                    </tr>
                    <tr className="hover:bg-gray-50">
                      <td className="p-3 font-medium">Case study development</td>
                      <td className="p-3 text-green-700">Claude</td>
                      <td className="p-3 text-gray-600">ChatGPT-4</td>
                    </tr>
                    <tr className="hover:bg-gray-50">
                      <td className="p-3 font-medium">Current events integration</td>
                      <td className="p-3 text-green-700">Perplexity AI</td>
                      <td className="p-3 text-gray-600">Gemini</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>

            <div className="grid md:grid-cols-2 gap-6">
              <div className="bg-blue-50 border border-blue-300 rounded-lg p-6">
                <h3 className="text-lg font-bold text-blue-900 mb-3">Budget-Conscious Approach</h3>
                <p className="text-blue-800 mb-3">Maximize free tiers:</p>
                <ul className="space-y-2 text-blue-700">
                  <li>• <strong>ChatGPT Free:</strong> Quick tasks, basic questions</li>
                  <li>• <strong>Claude Free:</strong> Detailed content when needed</li>
                  <li>• <strong>Perplexity Free:</strong> Research and fact-checking</li>
                  <li>• <strong>Strategy:</strong> Rotate tools to stay within limits</li>
                </ul>
              </div>

              <div className="bg-purple-50 border border-purple-300 rounded-lg p-6">
                <h3 className="text-lg font-bold text-purple-900 mb-3">Premium Investment Strategy</h3>
                <p className="text-purple-800 mb-3">If choosing one paid tool:</p>
                <ul className="space-y-2 text-purple-700">
                  <li>• <strong>Claude Pro:</strong> Best for humanities and detailed writing</li>
                  <li>• <strong>ChatGPT Plus:</strong> Most versatile, includes images</li>
                  <li>• <strong>Perplexity Pro:</strong> Best for research-heavy fields</li>
                  <li>• <strong>Consider:</strong> Your primary use case and discipline</li>
                </ul>
              </div>
            </div>

            <div className="bg-yellow-50 border border-yellow-400 rounded-lg p-6">
              <h3 className="text-xl font-bold text-yellow-900 mb-3">Ethical Considerations</h3>
              <div className="space-y-3 text-yellow-900">
                <div>
                  <h4 className="font-semibold mb-1">Transparency with Students:</h4>
                  <p className="text-yellow-800">Consider informing students about your AI use. Models: "I use AI to help structure lectures but all content is verified by me" or disclose specific AI-assisted materials.</p>
                </div>
                <div>
                  <h4 className="font-semibold mb-1">Data Privacy:</h4>
                  <p className="text-yellow-800">Never input student data, grades, or personally identifiable information into AI tools. Use anonymized examples only.</p>
                </div>
                <div>
                  <h4 className="font-semibold mb-1">Academic Integrity:</h4>
                  <p className="text-yellow-800">Model good AI use for students. Show them AI is a tool for enhancement, not replacement of critical thinking.</p>
                </div>
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
};

export default LectureToolkit;