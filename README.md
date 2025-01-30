A set of three files that work together:
  1. Pipe for OpenWebUI to allow chatting with an n8n workflow: https://github.com/yupguv/openwebui/blob/main/n8n_pipe_for_openwebui
  2. Demo n8n workflow: https://github.com/yupguv/openwebui/blob/main/n8n_workflow_openwebui_chat
  3. Table definitions if you want to use Supabase nodes in the demo n8n workflow: https://github.com/yupguv/openwebui/blob/main/openwebui_n8n_supabase_tables

- create an account here: https://webui.demodomain.dev/ to test the demo of this OpenWebUI pipe chatting with a live n8n workflow
  - or log in using null@void.com, with a password of 12345
  - If you do create an account, you can use a fake email, and if you use your real one, I won't spam you or anything stupid

Combined, the three elements (OpenWebUI pip, n8n workflow, supabase database) allow the following:
- OpenWebUI pipe connects to n8n by sending user messages to an n8n webhook. Valves are:
    - URL for the main n8n webhook
    - URL for status check n8n webhook
    - pipe timeout (seconds)
    - delay between n8n status check (seconds)
    - bearer token for authorization to n8n
    - chatInput field value
    - output field value
- pipe sets a looping function (using asyncio) to call another n8n webhook, every 2 seconds to get status updates, and display the latest status in the UI
  - updates from n8n can be "status", "no update", or "error"
- handles errors gracefully and shuts down the function that checks for updates on a loop
- uses OpenWebUi's __metadata__.get("chat_id") for chat session management with n8n
- n8n workflow accepts incoming webhooks requests from OpenWebUI, updates the status, and returns a response
- bearer token is sent by OpenWebUI and n8n verifies token
- workflow connects to a supabase demo database if you toggle the first node after the webhook to suse_supabase = true, otherwise it uses hard coded values for this demo
- creates a collapsable element containing COT (Chain of Thought) <thinking> tags above the final message

-----
- author: demodomain.dev
- author_url: https://github.com/yupguv
- version: 1.0
- license: MIT
