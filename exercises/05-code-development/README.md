# Module 05: Building an AI-Powered Application

This directory contains the exercise materials for Module 2.1 of the "Private LLM as a Service" workshop.

## Overview

Students will complete a partially-built Flask web application that manages tasks with AI-powered suggestions and analysis. The exercise teaches:

- AI-assisted development using Continue.dev
- Full-stack web application development with Flask
- Integration of LLM APIs into applications
- Building and deploying applications on OpenShift

## Application Structure

```
05-code-development/
├── app.py                 # Main Flask application (INCOMPLETE - students complete this)
├── requirements.txt       # Python dependencies
├── templates/
│   └── index.html        # Frontend UI (complete)
├── Dockerfile            # Container build configuration
└── openshift/            # OpenShift/Kubernetes manifests
    ├── buildconfig.yaml  # Builds container from Git
    ├── imagestream.yaml  # Tracks image versions
    ├── deployment.yaml   # Application deployment
    ├── service.yaml      # Internal networking
    └── route.yaml        # External access
```

## Incomplete Functions

Students need to implement these functions in `app.py` using Continue.dev:

1. **`get_tasks()`** - Return all tasks sorted by creation date
2. **`create_task()`** - Create a new task from JSON input
3. **`update_task()`** - Update an existing task
4. **`delete_task()`** - Delete a task by ID
5. **`call_ai_model()`** - Call the AI model API (OpenAI-compatible)
6. **`suggest_improvements()`** - Get AI suggestions for a specific task
7. **`analyze_tasks()`** - Use AI to analyze all tasks with different analysis types

## Learning Objectives

By completing this exercise, students will:

- Experience AI-assisted coding workflows
- Understand how to integrate LLMs into web applications
- Learn to construct effective prompts for AI models
- Deploy containerized applications on OpenShift
- Work with OpenShift build pipelines and image management

## Prerequisites

- OpenShift Dev Spaces environment with Continue.dev configured (from Module 04)
- Access to deployed Granite model (from Module 01)
- OpenShift CLI (`oc`) access
- Basic understanding of Python and REST APIs

## Testing Locally

Before deploying to OpenShift, students can test locally:

```bash
# Set environment variables
export AI_MODEL_URL="http://granite-instruct-vllm.<namespace>.svc.cluster.local:8000/v1/chat/completions"
export AI_MODEL_NAME="granite-3.0-8b-instruct"
export PORT=8080

# Install dependencies
pip install -r requirements.txt

# Run the application
python app.py
```

## Deployment to OpenShift

Students deploy using the provided manifests:

```bash
# Create project
oc new-project <username>-task-manager

# Deploy resources
oc apply -f openshift/imagestream.yaml
oc apply -f openshift/buildconfig.yaml
oc start-build task-manager --follow
oc apply -f openshift/deployment.yaml
oc apply -f openshift/service.yaml
oc apply -f openshift/route.yaml

# Get application URL
oc get route task-manager -o jsonpath='{.spec.host}'
```

## Expected Outcomes

A fully functional web application that:

- Creates, reads, updates, and deletes tasks
- Uses AI to generate context-aware suggestions for tasks
- Analyzes all tasks to provide prioritization and timeline insights
- Runs as a containerized application on OpenShift
- Demonstrates proper cloud-native practices (health checks, environment configuration)

## AI Model Integration

The application expects an OpenAI-compatible chat completion API with this format:

**Request:**
```json
{
  "model": "granite-3.0-8b-instruct",
  "messages": [
    {"role": "user", "content": "Your prompt here"}
  ],
  "max_tokens": 500,
  "temperature": 0.7
}
```

**Response:**
```json
{
  "choices": [
    {
      "message": {
        "content": "AI-generated response"
      }
    }
  ]
}
```

## Common Issues

### Build Failures
- Verify BuildConfig repository URL and branch
- Check that `contextDir` points to `exercises/05-code-development`

### AI Integration Not Working
- Verify `AI_MODEL_URL` environment variable in deployment.yaml
- Ensure the AI model service is accessible from the application namespace
- Check for network policies that might block inter-namespace communication

### Application Crashes
- Review pod logs: `oc logs deployment/task-manager`
- Verify all TODO functions are properly implemented
- Check for syntax errors in Python code

## Instructor Notes

### Time Estimate
- Code completion: 30-40 minutes
- Testing and debugging: 10-15 minutes
- Deployment: 15-20 minutes
- **Total: 55-75 minutes**

### Key Teaching Points

1. **AI-Assisted Development**: Emphasize that Continue.dev is a productivity tool, not a replacement for understanding code
2. **Prompt Engineering**: Show how clear, specific prompts yield better results
3. **Code Review**: Students should always review and understand AI-generated code
4. **Iteration**: Demonstrate refining prompts when initial results aren't perfect
5. **Cloud-Native**: Highlight 12-factor app principles (environment config, stateless design, health checks)

### Assessment Criteria

- All functions properly implemented
- Code handles errors gracefully
- Application deploys successfully to OpenShift
- AI integration works correctly with the model
- Students can explain how the code works

## License

This exercise is part of the Red Hat OpenShift AI workshop materials.
