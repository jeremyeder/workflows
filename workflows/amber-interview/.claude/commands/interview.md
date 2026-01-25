# /interview

Conduct a structured conversation to understand user needs.

## How It Works

1. **Consent** - Explain this will be saved and get permission
2. **Chat** - Ask exactly 3 questions about their experience
3. **Check-in** - 4th question: "Are you good to send this feedback, or do you have any final thoughts?"
4. **Summarize** - Pull together what we learned
5. **Show** - Let them review what will be shared
6. **Send** - They choose to send it or go back

**Question Limit:** Keep interviews focused by asking exactly 3 open-ended questions, then checking if they're ready to proceed with a 4th confirmation question.

## Privacy Notice (shown first)

"This conversation will be saved so we can learn from your feedback. Is that okay with you?"

## Example Flow

**After 3 questions:**
"Are you good to send this feedback, or do you have any final thoughts?"

If they share final thoughts → incorporate and continue to summary
If they're ready → proceed to summary

## What Users See Before Sending

```
Here's what we'll share with the team:

About: Debugging takes too much time
Category: Idea

What we learned:
- You check 3-5 different places for one error
- Hard to connect the dots between logs

[Full conversation]

Want to send this? (yes/no)
```

## What Gets Sent

```json
{
  "type": "interview",
  "content": {
    "transcript": "conversation",
    "summary": "main takeaway",
    "findings": ["what we learned"]
  },
  "metadata": {
    "tags": ["keywords"],
    "category": "idea"
  }
}
```

Sent to the team for review and action.
