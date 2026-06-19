# 💬 GD, HR & Troubleshooting — COMPLETE Interview Guide
### GreyOrange Software Support Engineer Intern | Drive: June 23-24, 2026

---

# SECTION A: COMPANY KNOWLEDGE (Use to impress in GD + Interviews)

## GreyOrange Quick Facts
* **Founded**: 2011 by Samay Kohli and Akash Gupta.
* **HQ**: Atlanta, Georgia, USA. Main Indian operations hub is in Gurugram, Haryana.
* **Core Business**: AI-driven robotic automation software and hardware, transforming distribution and global fulfillment centers.
* **Global Footprint**: Currently operating more than 70 fulfillment sites globally, with deployments of up to 700+ robots at a single site.
* **Key Clients**: DHL, DTDC, Myntra, Flipkart, Walmart (global logistics and retail leaders).

## Key Products & Technologies
* **AMR (Autonomous Mobile Robots)**:
  * Flagship series: **Ranger** (Ranger 8, Ranger GTP, Ranger Move).
  * Navigation: Navigate dynamically using LiDAR, cameras, SLAM algorithms, and onboard AI to replan paths around obstacles in real-time.
  * **AMR vs AGV**: AMRs are far superior to older **AGVs (Automated Guided Vehicles)** which can only navigate along fixed magnetic tapes or wires laid on the warehouse floor.
* **Butler System**:
  * A Goods-to-Person (G2P) solution where robots carry inventory storage PODs directly to human picking operators, eliminating workers walking miles through warehouse aisles.
* **GreyMatter (Multiagent Orchestration Platform - MAOP)**:
  * The proprietary AI execution software layer — the "brain" of the entire system. It coordinates robots, forecasts order demand, and optimizes storage layouts in real-time.
* **gStore**:
  * An end-to-end store execution and retail management solution using real-time overhead RFID technology. It increases retail store inventory accuracy up to 99% and doubles staff productivity.

## Industry Terminology (Drop these in conversations)
* **WMS**: Warehouse Management System (software managing inventory, stock, and orders).
* **WCS**: Warehouse Control System (lower-level software controlling conveyor belts and sorter hardware).
* **G2P**: Goods-to-Person (automation brings products directly to the worker).
* **SLA**: Service Level Agreement (legally binding response/resolution time commitments).
* **SRE**: Site Reliability Engineering (discipline of maintaining production system uptime and stability).
* **L1/L2/L3**: Support tiers (L1 = triage & first contact, L2 = technical investigation, L3 = advanced troubleshooting/engineering escalation).
* **MTTR**: Mean Time to Resolve (average time taken to restore services after an outage).
* **Throughput**: Volume of inventory items processed or sorted per hour.
* **SKU**: Stock Keeping Unit (unique identifier for each distinct product variant).

---

# SECTION B: SELF-INTRODUCTION SCRIPT (Rehearse 5+ times aloud)

*Target duration: 90 seconds. Speak at a moderate, steady pace. Do not read or sound robotic — internalize this structured script.*

--- SCRIPT ---

"Good morning. I'm Ravi, a third-year B.Tech CSE student at Lovely Professional University, graduating in May 2027.

My core technical strengths include Python, SQL, and full-stack web development with React and Node.js. I've also developed practical experience with Linux environments, REST APIs, and working with cloud platforms like Oracle Cloud and IBM Watsonx.

In terms of hands-on experience, I completed internships through IBM SkillsBuild focusing on AI and cloud hosting via AICTE, and through UptoSkills on satellite communications. On the project side, I've built multiple applications, including Devliq — a full-stack CS learning platform using Next.js, PostgreSQL, and AI-driven tutoring features — and a financial news sentiment analysis system that integrates NLP models with a FastAPI backend.

What draws me specifically to GreyOrange is the opportunity to work on enterprise-scale production systems. Supporting the GreyMatter platform as it coordinates hundreds of Ranger AMRs is exactly the kind of complex distributed system where I want to apply my troubleshooting skills and Linux/SQL knowledge.

I am fully comfortable with rotational shifts, passionate about global warehouse automation, and excited to contribute to GreyOrange's support operations. Thank you."

--- END SCRIPT ---

### ⚠️ What NOT to say:
* ❌ **"I am a very hardworking, honest person"** (Too generic — show your qualities through projects instead).
* ❌ **"I love learning new things"** (Overused filler phrase).
* ❌ **"I don't have much experience because I'm a fresher"** (Avoid self-deprecation; your internships and projects are valuable experience).
* ❌ **Reciting details word-for-word from memory** (Interviewer will spot it. Speak conversationally using the bulleted structure).
* ❌ **Going beyond 2 minutes** (Keep it under 90 seconds to hold their attention).

---

# SECTION C: GROUP DISCUSSION (GD) MASTER CLASS

The GD is the primary elimination round on Day 1. The key is not to dominate or shout, but to demonstrate structured thinking, active listening, and leadership.

## 1. The PREP Method (Your Structure for Every Turn)
* **P = Point**: State your position or viewpoint clearly in 15 seconds.
* **R = Reason**: Explain the logical reason behind your point in 20 seconds.
* **E = Example**: Give a concrete, real-world example (preferably related to tech/GreyOrange) in 20 seconds.
* **C = Conclusion**: Reiterate your point with impact in 10 seconds.

## 2. GD Role Scripts (How to Start, Pivot, and Summarize)

### 🎙️ How to INITIATE (Start the GD)
Only initiate if you understand the topic well. Starting gives you massive visibility.
> *"Good morning friends. Today we have been given the topic [Topic Name]. I'd like to initiate our discussion by pointing out that... [Use PREP]. Let's open the floor to hear everyone's perspectives."*

### 🤝 How to BUILD (Acknowledge and Add)
Use this to enter the discussion politely without interrupting or shouting.
> *"I completely agree with the point made by [Name] regarding [Point]. Building on that, another angle we must consider is... [Use PREP]."*

### 🔄 How to PIVOT (Correct Course)
Use this if the group is going off-topic or arguing. The moderators love candidates who bring the group back to the core topic.
> *"While we have had a great discussion about [Current Point], I would like to steer us back to the core question of the topic, which is [Topic Topic]. Let's focus on..."*

### 📋 How to SUMMARIZE (Close the GD)
Do this at the end of the time limit. Never introduce new points in the summary.
> *"Since we are nearing the end of our time limit, I would like to summarize the main viewpoints of our group. We discussed that while [Point A] is true, [Point B] represents the primary challenge, and we agree that [Conclusion] is the way forward. Thank you."*

---

## 3. Top 6 GD Topics & Talking Points

### Topic 1: AI and Automation Replacing Jobs
* **Point**: Automation shifts the nature of human labor; it does not eliminate it entirely.
* **Reason**: Automation replaces repetitive, physically dangerous, and exhausting tasks, creating new needs for highly skilled roles to deploy, monitor, and maintain the systems.
* **Example**: GreyOrange AMRs automate picking walks in warehouses, but this creates a global demand for Software Support and SRE teams to maintain the GreyMatter orchestrator.
* **Conclusion**: The focus should be on reskilling workforces to adapt to Industry 4.0.

### Topic 2: Work From Home (WFH) vs Office
* **Point**: For hardware-integrated enterprise software support, a hybrid model is superior to fully remote work.
* **Reason**: Critical production outages (P1) require immediate, high-bandwidth team collaboration. War rooms in offices often resolve issues faster, while routine monitoring can be handled remotely.
* **Example**: Support teams working globally across rotational shifts can coordinate handovers better via structured hybrid workspaces.
* **Conclusion**: Hybrid setups offer flexibility while preserving collaborative efficiency for incident response.

### Topic 3: India as a Global Technology Hub
* **Point**: India is transitioning from back-office IT outsourcing to global product development and R&D leadership.
* **Reason**: India has a massive engineering pool, strong English proficiency, and a booming startup ecosystem, attracting product companies to base their core engineering here.
* **Example**: GreyOrange was founded in India and maintains a major R&D hub in Gurugram, building systems deployed in major warehouses in North America and Europe.
* **Conclusion**: Indian talent is now designing global products, not just maintaining them.

### Topic 4: Importance of Communication in Technical Roles (HIGH PRIORITY)
* **Point**: A technically perfect fix is incomplete without clear, timely communication.
* **Reason**: Software Support is client-facing. If a critical warehouse is down, client anxiety is high. Transparent updates build trust and keep the client calm during resolution.
* **Example**: Explaining "We are restarting database connection pools, expecting restoration in 10 minutes" is far better than silence or saying "We are working on it."
* **Conclusion**: Modern technical roles require a balance of technical excellence and structured, empathetic communication.

### Topic 5: Startup Culture vs Corporate Jobs
* **Point**: For freshers, a funded product firm like GreyOrange offers the ideal mix of startup agility and corporate structure.
* **Reason**: Agility gives you high ownership and rapid exposure to multiple technologies. Corporate structure ensures standard operating procedures (SOPs) are in place.
* **Example**: Having L1 -> L2 -> L3 support escalation structures provides freshers with clear learning paths while allowing them to troubleshoot real outages.
* **Conclusion**: Freshers should prioritize environments that offer both high ownership and structured guidance.

### Topic 6: Ethical Use of AI in Automation
* **Point**: As AI automates physical and digital processes, algorithmic transparency and fairness are critical.
* **Reason**: AI models are trained on historical data. If data contains biases (e.g. favoring specific shipping routes or inventory types), the AI will perpetuate them.
* **Example**: Demand forecasting algorithms must be monitored to ensure equitable delivery speeds and stock allocations across all client sectors.
* **Conclusion**: Ethical AI requires continuous audit trails and diverse datasets.

---

# SECTION D: HR INTERVIEW Q&A

### Q1: Why GreyOrange?
* **Answer**: *"GreyOrange is a global pioneer in warehouse robotics and AI fulfillment. What excites me is your GreyMatter Multiagent Orchestration Platform. As a computer science student with a strong interest in Linux, database operations, and Python scripting, the opportunity to support complex distributed systems that coordinate hundreds of robots in real-time is the perfect place to start my career. I want to work close to the system level and help maintain enterprise-scale logistics uptime."*

### Q2: Why Software Support instead of Core Development?
* **Answer**: *"I enjoy real-time problem solving and understanding systems as a whole. Software Support is a blend of Systems Engineering, SRE, and Database operations. Developers write code in isolated environments, but Support Engineers see how code behaves under real production load with real edge cases. Troubleshooting server crashes, database latencies, and network drops builds a very deep, practical engineering foundation."*

### Q3: Are you comfortable with 24x7 rotational shifts and night shifts?
* **Answer**: *"Yes, absolutely. I understand that GreyOrange supports global client operations across multiple time zones. I am highly adaptable, maintain good time-management habits, and am fully prepared to work night shifts and weekend rotations to ensure our client installations have 100% operational uptime."*

### Q4: Tell me about a time you handled a conflict in a team. (STAR Method)
* **S = Situation**: *"In our B.Tech CSE project, our team of four was divided. Two members wanted to use Java for our backend, while I and another member advocated for Python to build our analytics API."*
* **T = Task**: *"As the project coordinator, I had to resolve this disagreement without delaying our prototype submission deadline."*
* **A = Action**: *"I scheduled a brief meeting where both sides demonstrated a quick proof-of-concept. I showed that while Java is extremely robust, Python allowed us to write 60% less code for the machine learning integration. I suggested a compromise: we use Python for the API service and Java for our desktop client component."*
* **R = Result**: *"The team agreed. We completed the project 3 days ahead of schedule, achieved an 'A' grade, and learned to resolve conflicts using data rather than arguments."*

### Q5: What is your greatest strength and weakness?
* **Strength**: *"My greatest strength is structured troubleshooting under pressure. When an incident occurs, I do not panic or make random changes. I systematically check application logs, check system resource metrics (CPU, memory, disk), and isolate the root cause before applying the fix."*
* **Weakness**: *"I sometimes find it hard to delegate tasks because I want to ensure they are done perfectly, which leads to working long hours. To address this, I've started practicing task breakdown and trust-building with my teammates, which has helped me manage collaborative workloads much better."*

### Q6: Where do you see yourself in 3 years?
* **Answer**: *"In 3 years, I see myself progressing into a Senior Support Engineer or SRE role, having gained deep technical expertise in GreyOrange's GreyMatter and Ranger product lines. I also want to contribute to our internal tools by writing automation scripts to reduce MTTR. Ultimately, I want to bridge the gap between support and product engineering, using production incident data to help make our core software more resilient."*

### Q7: Are you comfortable with the offered stipend of ₹25,000/month during the internship and ₹7.5 LPA PPO?
* **Answer**: *"Yes, I am fully comfortable with the stipend and the CTC structure. The opportunity to work on cutting-edge warehouse robotics software and gain hands-on production engineering experience at GreyOrange is my primary motivation. I am fully committed to converting this internship into a full-time role."*

### Q8: What do you know about GreyOrange's products?
* **Answer**: *"Your flagship product is the Ranger series of Autonomous Mobile Robots (AMRs), which use LiDAR, SLAM, and AI to navigate warehouses dynamically (unlike older AGVs that follow fixed floor tape). You also have the Butler System, which is a Goods-to-Person solution bringing inventory storage pods directly to pickers. Everything is coordinated by the GreyMatter AI platform, which acts as the system's brain, optimizing paths and predicting demand. Lastly, gStore uses RFID to maintain 99% inventory accuracy in retail stores."*

### Q9: What will you do if your shift partner doesn't show up during a major P1 outage?
* **Answer**: *"If my shift partner does not show up during an active P1 outage, my priority is the system's stability and the client's operations. I will stay online, continue working on the issue, and immediately notify my manager of the situation. I will not log off until the outage is resolved or a formal handover has been conducted with the incoming shift engineer. System uptime and client support always come first."*

### Q10: How do you handle stress or long troubleshooting sessions?
* **Answer**: *"I handle stress by relying on structured processes. I break complex issues into smaller diagnostic steps: checking network connectivity first, then database pools, and then application logs. I also keep a log of my steps so I don't repeat them. If a session is very long, I ensure I take brief 1-minute stretches, stay hydrated, and keep my manager updated on progress to maintain clarity."*

---

# SECTION E: DRESS CODE & PHYSICAL DOCUMENTS CHECKLIST

## Dress Code (Strictly Formal)
* **Gentlemen**: Solid light-colored formal shirt (white or light blue), dark formal trousers (navy, charcoal, black), dark leather belt, and formal leather shoes (strictly NO sneakers, NO jeans, NO casual shirts). Clean-shaven or neatly groomed beard. Tie is highly recommended.
* **Ladies**: Formal shirt with trousers, or formal Indian suit. Neat hair and formal shoes/flats.

## Documents Folder Structure
Prepare two separate physical folders for the drive:

### Folder 1: ATS Resume & Photos
* [ ] 2 printed copies of your ATS-optimized resume.
* [ ] 2 recent passport-size photographs (formal background).

### Folder 2: Academic & Identification Documents
* [ ] 10th marksheet and passing certificate.
* [ ] 12th marksheet and passing certificate.
* [ ] B.Tech semester transcripts (all completed semesters).
* [ ] College ID Card (LPU Card) — mandatory for entry.
* [ ] Government ID: Aadhaar Card or PAN Card.
