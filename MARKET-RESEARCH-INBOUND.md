Here is the complete Automated Market Research Solution built with Make.com, Groq AI, and Google Sheets.
When a lead registers or is tagged with Market Research (under Strategic Marketing), this automation generates an Account & Industry Market Research Dossier so your consulting or sales team can walk into discovery calls fully prepared.

End-to-End Workflow
[Router 18 / New Lead Event] 
       ↓
[1. Make.com Filter] (Triggered if Pillar = "Strategic Marketing" & Sub-item = "Market Research")
       ↓
[2. Groq AI: Market Research Analyst] (Executes Market Sizing, Trends, ICP, Competitors, and GTM Angles)
       ↓
[3. Parse JSON] (Converts Groq analysis into discrete columns)
       ↓
[4. Google Sheets: Append Row] (Saves to dedicated "Market Research Dossiers" sheet)
       ↓
[5. Slack / Email Notification] (Sends 1-click briefing to the Account Executive)



1. Groq Module Configuration (System & User Prompts)
Module in Make.com: Groq: Create a JSON Chat Completion
Model: llama-3.3-70b-versatile
Response Format: JSON Object


System Prompt:
You are a Principal Market Research Consultant at B3 Consulting. You conduct strategic market analysis on target companies and their industries.
Analyze the submitted company, their market segment, and competitors, and return strictly one valid JSON object. No markdown code blocks, backticks, or outer text.
### RESEARCH METHODOLOGY & FRAMEWORK
1. Industry & Macro Landscape: Identify their primary sector, market tailwinds, and regulatory/technological shifts.
2. Target Audience & Buyer Persona: Who their primary decision-makers are and their core friction points.
3. Competitor Ecosystem: Name 2-3 primary direct/indirect market rivals and where the target company sits in market maturity.
4. Strategic Market Opportunities: Unmet customer needs, emerging white spaces, or underserved niches.
5. B3 Consulting Pitch Angle: A concrete consulting angle on how B3 can assist this client in conducting primary research, audience segmentation, or market validation.

### OUTPUT JSON SCHEMA
{
  "company_name": "string",
  "industry_vertical": "string",
  "market_overview": "2 concise sentences on market size, maturity, and growth trajectory",
  "key_market_trends": [
    "Trend 1 with brief business implication",
    "Trend 2 with brief business implication",
    "Trend 3 with brief business implication"
  ],
  "target_buyer_personas": "Primary buying group and their operational pain points",
  "key_competitors": ["Competitor A", "Competitor B", "Competitor C"],
  "market_differentiator": "Their probable competitive edge or positioning challenge",
  "white_space_opportunity": "Where the market is moving and what gaps exist",
  "recommended_research_methodology": "e.g., Qualitative IDIs, Win/Loss Analysis, Conjoint Analysis, or B2B Customer Panel",
  "consulting_pitch_hook": "2-3 sentences tying their market landscape directly into a B3 Consulting advisory engagement"
}



User Prompt:
Generate a comprehensive Market Research dossier for this account:
Company: {{38.company_clean}}
Domain: {{38.domain_clean}}
Contact Title: {{38.standardized_title}}
Country: {{38.country_code}}
Event Source: B3 Strategic Marketing Webinar
Return only the JSON object.



2. Google Sheets Structure: "Market Research Dossiers" Tab
Create a dedicated tab in your Google Sheet with these headers:
Col	Header	Mapped Make.com Token (from Parse JSON)
A	Generated Date	{{now}}
B	Company	{{parse_json.company_name}}
C	Domain	{{38.domain_clean}}
D	Lead Contact	{{38.first_name}} {{38.last_name}} ({{38.standardized_title}})
E	Industry Vertical	{{parse_json.industry_vertical}}
F	Market Overview	{{parse_json.market_overview}}
G	Top Trends	{{join(parse_json.key_market_trends; " • ")}}
H	Key Competitors	{{join(parse_json.key_competitors; ", ")}}
I	White Space / Gap	{{parse_json.white_space_opportunity}}
J	Recommended Methodology	{{parse_json.recommended_research_methodology}}
K	Consulting Pitch Hook	{{parse_json.consulting_pitch_hook}}


3. Automated Executive Briefing (Slack / Email)
Add a Slack/Email step right after Google Sheets so your consultants get the dossier before contacting the lead:
Subject: 📊 Market Research Dossier: {{parse_json.company_name}} ({{38.first_name}} {{38.last_name}})
Industry: {{parse_json.industry_vertical}}
Overview: {{parse_json.market_overview}}
Key Rivals: {{join(parse_json.key_competitors; ", ")}}
Market Opportunity: {{parse_json.white_space_opportunity}}
Suggested Service: {{parse_json.recommended_research_methodology}}
Meeting Opener / Hook: {{parse_json.consulting_pitch_hook}}


This turns an incoming registration into an instant consulting briefing, eliminating 30–45 minutes of manual research per lead.