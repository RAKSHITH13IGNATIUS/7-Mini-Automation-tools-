{
  "name": "To do list",
  "nodes": [
    {
      "parameters": {
        "promptType": "define",
        "text": "={{ $json.text }}, REFERENCE_DATE = {{ $now.setZone('Asia/Kolkata').toFormat('yyyy-LL-dd') }}\n",
        "hasOutputParser": true,
        "messages": {
          "messageValues": [
            {
              "message": "=ROLE\nYou are “Todoist Task‑Builder”.\n\nREFERENCE_IST = {{ $now.setZone('Asia/Kolkata').toISO() }}\n\nINPUT  \nUser prompts in natural language  \n(may include phrases like “tomorrow at 10 p.m.”).\n\nOUTPUT  \nReturn **one** JSON array, nothing else.\n\n{\n  \"content\":        \"<concise action>\",\n  \"priority\":       <1‑4>,            // 4 = highest\n  \"due_date_time\":  \"<RFC 3339 timestamp with +05:30 offset or \\\"\\\">\"\n}\n\nRULES  \n1. Split the request into clear, doable sub‑tasks; avoid vague verbs.  \n2. Set **priority** by urgency and impact.  \n3. Interpret every date relative to **REFERENCE_IST** and convert to RFC 3339 **with the +05:30 offset**.  \n   • “tomorrow 10 p.m.” → 2025‑08‑01T22:00:00+05:30.  \n   • If no date, set \"due_date_time\" to \"\".  \n4. If the date or zone is unclear, ask one short follow‑up question, then stop.  \n5. Output must be valid JSON: no markdown, no prose, no extra keys, no trailing commas.\n\nEXAMPLE  \nUser →  \nI need to go to the gym tomorrow at 10 p.m. and it’s top priority.\n\nAssistant →  \n[\n  {\n    \"content\": \"Go to the gym.\",\n    \"priority\": 4,\n    \"due_date_time\": \"2025‑08‑01T22:00:00+05:30\"\n  }\n]\n"
            }
          ]
        }
      },
      "id": "90fc2d11-9b45-4ed6-b491-7fba9d3e891d",
      "name": "Basic LLM Chain",
      "type": "@n8n/n8n-nodes-langchain.chainLlm",
      "position": [
        480,
        0
      ],
      "typeVersion": 1.5
    },
    {
      "parameters": {
        "updates": [
          "message"
        ],
        "additionalFields": {}
      },
      "id": "8581204e-040e-4ca1-9772-fd6fa93ab6d3",
      "name": "Receive Telegram Messages",
      "type": "n8n-nodes-base.telegramTrigger",
      "position": [
        -496,
        144
      ],
      "webhookId": "4e2cd560-ae4e-4ed7-a8ea-984518404e51",
      "typeVersion": 1.1,
      "credentials": {
        "telegramApi": {
          "id": "qH9d104CH7bXxbHe",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "version": 2,
                  "leftValue": "",
                  "caseSensitive": true,
                  "typeValidation": "strict"
                },
                "combinator": "and",
                "conditions": [
                  {
                    "id": "af30c479-4542-405f-b315-37c50c4e2bef",
                    "operator": {
                      "type": "string",
                      "operation": "exists",
                      "singleValue": true
                    },
                    "leftValue": "={{ $json.message.voice.file_id }}",
                    "rightValue": ""
                  }
                ]
              },
              "renameOutput": true,
              "outputKey": "Audio"
            },
            {
              "conditions": {
                "options": {
                  "version": 2,
                  "leftValue": "",
                  "caseSensitive": true,
                  "typeValidation": "strict"
                },
                "combinator": "and",
                "conditions": [
                  {
                    "id": "a3ca8cd4-fbb2-40b5-829a-24724f2fbc85",
                    "operator": {
                      "type": "string",
                      "operation": "exists",
                      "singleValue": true
                    },
                    "leftValue": "={{ $json.message.text || \"\" }}",
                    "rightValue": ""
                  }
                ]
              },
              "renameOutput": true,
              "outputKey": "Text"
            },
            {
              "conditions": {
                "options": {
                  "version": 2,
                  "leftValue": "",
                  "caseSensitive": true,
                  "typeValidation": "strict"
                },
                "combinator": "and",
                "conditions": [
                  {
                    "id": "9bcfdee0-2f09-4037-a7b9-689ef392371d",
                    "operator": {
                      "type": "string",
                      "operation": "exists",
                      "singleValue": true
                    },
                    "leftValue": "error",
                    "rightValue": ""
                  }
                ]
              },
              "renameOutput": true,
              "outputKey": "Error"
            }
          ]
        },
        "options": {}
      },
      "id": "24e1cdd5-b134-4495-b42c-f029a213b7a8",
      "name": "Voice or Text?",
      "type": "n8n-nodes-base.switch",
      "position": [
        -256,
        128
      ],
      "typeVersion": 3.2
    },
    {
      "parameters": {
        "resource": "file",
        "fileId": "={{ $json.message.voice.file_id }}",
        "additionalFields": {}
      },
      "id": "2e63a376-e699-4953-a2bf-7ada42eac364",
      "name": "Fetch Voice Message",
      "type": "n8n-nodes-base.telegram",
      "position": [
        0,
        0
      ],
      "webhookId": "23645237-4943-4c32-b18c-97c410cc3409",
      "typeVersion": 1.2,
      "credentials": {
        "telegramApi": {
          "id": "qH9d104CH7bXxbHe",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "resource": "audio",
        "operation": "translate",
        "options": {}
      },
      "id": "f966630d-4e1c-4a22-b2d3-ced27c4a3ef4",
      "name": "Transcribe Voice to Text",
      "type": "@n8n/n8n-nodes-langchain.openAi",
      "position": [
        224,
        0
      ],
      "typeVersion": 1.8,
      "credentials": {
        "openAiApi": {
          "id": "72G88X0R15TLaQRU",
          "name": "OpenAi account"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "d26209a2-24c0-4c7b-bf3b-7790ca1ba394",
              "name": "",
              "value": "",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "id": "250a861c-08c9-4969-85c5-da085810b9b1",
      "name": "Prepare for LLM",
      "type": "n8n-nodes-base.set",
      "position": [
        112,
        224
      ],
      "typeVersion": 3.4
    },
    {
      "parameters": {
        "schemaType": "manual",
        "inputSchema": "{\n  \"$schema\": \"http://json-schema.org/draft-07/schema#\",\n  \"type\": \"array\",\n  \"items\": {\n    \"type\": \"object\",\n    \"properties\": {\n      \"content\":        { \"type\": \"string\" },\n      \"priority\":       { \"type\": \"integer\", \"minimum\": 1, \"maximum\": 4 },\n      \"due_date_time\":  { \"type\": \"string\" }\n    },\n    \"required\": [\"content\", \"priority\", \"due_date_time\"],\n    \"additionalProperties\": false\n  }\n}\n",
        "autoFix": true
      },
      "id": "80ac217d-5b8a-49e8-98f9-2d1156d11cf0",
      "name": "Extract Tasks",
      "type": "@n8n/n8n-nodes-langchain.outputParserStructured",
      "position": [
        544,
        288
      ],
      "typeVersion": 1.2
    },
    {
      "parameters": {
        "project": {
          "__rl": true,
          "value": "2358420317",
          "mode": "list",
          "cachedResultName": "n8n demo"
        },
        "labels": "={{ [] }}",
        "content": "={{ $json.output[0].content }}",
        "options": {
          "dueDateTime": "={{ $json.output[0].due_date_time }}",
          "priority": "={{ $json.output[0].priority }}"
        }
      },
      "id": "90c60480-d315-495c-9ebd-e74d1a49934f",
      "name": "Create Todoist Tasks",
      "type": "n8n-nodes-base.todoist",
      "position": [
        832,
        -16
      ],
      "typeVersion": 2.1,
      "credentials": {
        "todoistApi": {
          "id": "voBtNOGzd6EdX4r0",
          "name": "Todoist account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Receive Telegram Messages').item.json.message.chat.id }}",
        "text": "=Task : {{ $json.content }} Task Link :{{ $json.url }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "id": "d9507d79-ebde-4bd1-a7b0-e1d1a927dd5a",
      "name": "Send Confirmation",
      "type": "n8n-nodes-base.telegram",
      "position": [
        1056,
        -16
      ],
      "webhookId": "5699aecd-e061-4b7f-af7b-4a23eb7201c6",
      "typeVersion": 1.2,
      "credentials": {
        "telegramApi": {
          "id": "qH9d104CH7bXxbHe",
          "name": "Telegram account"
        }
      }
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [
        320,
        288
      ],
      "id": "60ae0e25-3fcd-404b-b6dd-d3d290c7a4ed",
      "name": "Google Gemini Chat Model",
      "credentials": {
        "googlePalmApi": {
          "id": "MiYRVJsW5RITQKw4",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    }
  ],
  "pinData": {},
  "connections": {
    "Extract Tasks": {
      "ai_outputParser": [
        [
          {
            "node": "Basic LLM Chain",
            "type": "ai_outputParser",
            "index": 0
          }
        ]
      ]
    },
    "Voice or Text?": {
      "main": [
        [
          {
            "node": "Fetch Voice Message",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Prepare for LLM",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Basic LLM Chain": {
      "main": [
        [
          {
            "node": "Create Todoist Tasks",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Prepare for LLM": {
      "main": [
        [
          {
            "node": "Basic LLM Chain",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Fetch Voice Message": {
      "main": [
        [
          {
            "node": "Transcribe Voice to Text",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Create Todoist Tasks": {
      "main": [
        [
          {
            "node": "Send Confirmation",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Transcribe Voice to Text": {
      "main": [
        [
          {
            "node": "Basic LLM Chain",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Receive Telegram Messages": {
      "main": [
        [
          {
            "node": "Voice or Text?",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Gemini Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "Basic LLM Chain",
            "type": "ai_languageModel",
            "index": 0
          },
          {
            "node": "Extract Tasks",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1"
  },
  "versionId": "cdc2de72-27a5-4b8f-98d3-9e8230737694",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "30381347555293bc7636ec78fa7cb36851184bc2a2e942135a1daaf8692f9ebf"
  },
  "id": "OXq26frGNlVMEbqs",
  "tags": []
}
