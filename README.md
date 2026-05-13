## TW Loan Acquisition Chatbot PART 1

### TVS Loan Recommendation Model

This model provides a customer-friendly chatbot taking minimum questions from users, processes using ML pipeline and provides an instant loan eligibility decision (approve/decline).

By further integrating the conversational UI with a personalized recommendation layer, where instead of rejecting a customer outright for the specific model, it evaluates all models under the given Make_Code and returns the list of approved/eligible options for the user.

This approach softens the impact of rejection, gives user multiple financing choices instantly, improves satisfaction & increases likelihood of conversion.

### Basic Overview

  1. Problem Space : There exists fundamental tension between 2 critical objectives - **maximizing efficiency of loan application process** & **minimizing credit risk carried by new applicants**.
  2. Proposed Solution Diagram :
<div>
<img src = 'Architecture Diagram.png' height="350px">
</div>

### Detailed Overview

  1. User Input (9 Fields)
     - Age
     - Gender
     - Pincode
     - Qualifications
     - Employment_Type
     - Net_salary
     - Make_Code
     - Loan_Amount
     - PAST_LOANS_ACTIVE

  2. Lookup Table
     - PINCODE_MAP : Dictionary grouped by Pincode with State, State_Avg_Salary, Final_Tier (14031 entries)
     - MAKE_CODE_MAP : Dictionary of Make_Code mapped to Model_Description, Product_Code, Avg_Model_Price (13 entries) 

  3. Feature Engineering
     - Derived Features
       - Avg Model Price
       - Income Presence (Income / No Income / Guarantor)
     - Binning
       - Age Group
       - LTV Band (Low / Medium / High)
       - Income Tier (Low → Very High)
       - Loan Amount Band
     - Relational Features
       - Employment × Gender
       - Income-to-Loan Ratio
       - Age-to-Loan Ratio
       - Salary vs State Average
       - Past Loans × Employment
       - State-Pincode Zone

  4. Model Used : LightGBM (Gradient Boosted Decision Trees)
  5. Model Performance

| Metrics | Value |
| --- | --- |
| Accuracy | 0.9431 |
| Precision | 0.9458 |
| Recall | 0.9970 |
| F1 Score | 0.9707 |
| ROC AUC Score | 0.7651 *(more critical to separate safe borrowers from risky ones)* |
| KS Statistics | 0.4 *(in credit risk modeling, KS = 0.40 puts model in strong performance category)* |
| Processing Time | ~0.04 seconds *(reduced with real time feature engineering & table creation)* |
| Chat Session Time | ~26.63 seconds *(dependent on user)* |

### Risks & Trade-offs

| Risk | Description |
| --- | --- |
| False Positives | Higher NPAs (approving risky customers) |
| False Negatives | Losing eligible customers |

### Conclusion

This solution demonstrates how : 
  - **Conversational AI + ML + Recommendation Systems** can transform lending
  - Real-time eligibility can directly guide purchase decisions
  - Integrating approval with bike recommendations improves both UX and business outcomes

It effectively balances : Speed (customer experience), Accuracy (risk control) & Personalization (bike recommendations)
