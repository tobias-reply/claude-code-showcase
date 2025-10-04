# AgentCore Analysis: Strands Agent with Basic LLM Connection

## CRITICAL AgentCore Implementation Assessment

### Project Context Analysis
**Current State**: Fresh project with no existing code or dependencies
**Goal**: Scaffold AgentCore Strands agent for basic LLM connectivity testing
**Priority**: Foundation setup for AgentCore framework integration

## MANDATORY AgentCore Framework Requirements

### 1. AgentCore Strands Framework Setup

**⚠️ CRITICAL CLARIFICATION NEEDED**:
The session overview mentions "AgentCore Strands" as a framework. However, based on official AWS AgentCore documentation, there is NO framework called "AgentCore Strands" in the Amazon Bedrock AgentCore service suite.

**Available AgentCore Framework Options**:
1. **LangGraph Integration** - Official AgentCore-supported framework
2. **CrewAI Integration** - Native AgentCore framework support  
3. **Custom FastAPI Implementation** - Direct AgentCore Runtime integration
4. **Strands Agents Framework** - Third-party framework (requires custom integration)

**RECOMMENDATION**: Clarify framework choice before proceeding:
- If "Strands Agents" is intended: Custom integration pattern required with AgentCore Runtime
- If LangGraph/CrewAI desired: Use official AgentCore integrations
- If basic agent needed: Use AgentCore SDK approach

### 2. AgentCore Runtime Architecture

**Platform Requirements (NON-NEGOTIABLE)**:
- Linux/ARM64 platform mandatory for AgentCore Runtime
- Python 3.10+ required
- Container deployment to Amazon ECR
- Port 8080 exposure for agent communication
- FastAPI framework for HTTP interface

**Mandatory Endpoints**:
- `/invocations` (POST): Agent interaction endpoint
- `/ping` (GET): Health check endpoint

### 3. AgentCore SDK Implementation Path

**Recommended Approach for Basic Setup**:
```
Project Structure:
├── agent.py                 # Main agent implementation
├── requirements.txt         # Dependencies
├── Dockerfile              # ARM64 container
├── .env                    # Environment variables
└── tests/                  # Basic testing
```

**Core Dependencies**:
```
bedrock-agentcore>=1.0.0
fastapi>=0.104.0
uvicorn[standard]>=0.24.0
pydantic>=2.5.0
boto3>=1.34.0
python-dotenv>=1.0.0
```

### 4. AgentCore Identity Integration

**LLM Provider Authentication Patterns**:

**Option A: AWS Bedrock (Recommended)**
- Use AgentCore Identity service for secure token management
- IAM role-based authentication
- No API keys in code/environment

**Option B: External Providers (OpenAI/Anthropic)**
- AgentCore Identity token vault integration
- Secure credential storage without user consent fatigue
- OAuth token management through AgentCore

**CRITICAL SECURITY REQUIREMENT**: Never store API keys directly in code or plain environment variables when using AgentCore

### 5. Basic Agent Implementation Pattern

**AgentCore SDK Approach**:
```python
from bedrock_agentcore import BedrockAgentCoreApp

app = BedrockAgentCoreApp()

@app.entrypoint
def handle_request(event):
    # Basic LLM connection logic
    prompt = event.get('prompt', 'Hello, are you online?')
    # LLM API call
    # Return response
    return {"response": llm_response}
```

**Custom FastAPI Approach** (More Control):
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class InvocationRequest(BaseModel):
    prompt: str

@app.post("/invocations")
async def invoke_agent(request: InvocationRequest):
    # LLM connection and processing
    pass

@app.get("/ping")
async def health_check():
    return {"status": "healthy"}
```

### 6. AgentCore Deployment Configuration

**Container Requirements**:
- Base image: ARM64 compatible (e.g., `python:3.11-slim` for ARM64)
- Expose port 8080
- Include all dependencies
- Environment variable handling

**ECR Deployment Steps**:
1. Build ARM64 container: `docker buildx build --platform linux/arm64`
2. Push to ECR repository
3. Create AgentCore Runtime via API: `CreateAgentRuntime`
4. Configure network and IAM roles

### 7. LLM Integration Patterns

**AWS Bedrock Integration** (Recommended):
```python
import boto3
from botocore.exceptions import ClientError

def connect_bedrock_llm():
    try:
        bedrock_client = boto3.client('bedrock-runtime', region_name='us-east-1')
        # Test connection with basic invoke
        response = bedrock_client.invoke_model(
            modelId='anthropic.claude-3-sonnet-20240229-v1:0',
            body=json.dumps({
                "anthropic_version": "bedrock-2023-05-31",
                "max_tokens": 100,
                "messages": [{"role": "user", "content": "Hello, are you online?"}]
            })
        )
        return response
    except ClientError as e:
        # Handle authentication/permission errors
        raise
```

**External Provider Pattern**:
```python
import openai
from agentcore_identity import get_secure_token

def connect_external_llm():
    # Use AgentCore Identity for secure token management
    api_key = get_secure_token('openai_api_key')
    client = openai.OpenAI(api_key=api_key)
    
    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": "Hello, are you online?"}]
    )
    return response
```

### 8. AgentCore Observability Integration

**Mandatory Monitoring Setup**:
```python
from opentelemetry import baggage
from opentelemetry.instrumentation.auto_instrumentation import sitecustomize

# Session tracking for AgentCore
def set_session_context(session_id: str):
    baggage.set_baggage("session.id", session_id)

# Enable CloudWatch integration
# Install: aws-opentelemetry-distro>=0.10.1
# Run with: opentelemetry-instrument python agent.py
```

**CloudWatch GenAI Observability**:
- Automatic trace collection for LLM calls
- Token usage monitoring
- Latency and error rate tracking
- Session-level performance analysis

### 9. Local Development Setup

**AgentCore Starter Toolkit Approach**:
```bash
# Install AgentCore toolkit
pip install bedrock-agentcore-starter-toolkit

# Configure project
agentcore configure --entrypoint agent.py

# Local testing (requires Docker/Finch/Podman)
agentcore launch --local

# Deploy to AgentCore Runtime
agentcore launch

# Test invocation
agentcore invoke '{"prompt": "Hello, are you online?"}'
```

### 10. Testing and Validation Framework

**Basic Test Structure**:
```python
import pytest
from agent import handle_request

def test_basic_llm_connection():
    """Test basic LLM connectivity"""
    event = {"prompt": "Hello, are you online?"}
    response = handle_request(event)
    
    assert "response" in response
    assert len(response["response"]) > 0
    assert "online" in response["response"].lower()

def test_health_endpoint():
    """Test agent health check"""
    # Health check validation
    pass
```

## CRITICAL IMPLEMENTATION DECISIONS REQUIRED

### 1. Framework Choice Clarification
**URGENT**: Confirm intended framework:
- Official AgentCore-supported framework (LangGraph/CrewAI)
- Third-party Strands Agents framework (requires custom integration)
- Direct AgentCore SDK implementation

### 2. LLM Provider Selection
**Primary Options**:
- **AWS Bedrock**: Recommended for full AgentCore integration
- **OpenAI**: Requires AgentCore Identity integration
- **Anthropic**: Requires secure token management

### 3. Deployment Approach
**SDK vs Custom Implementation**:
- **SDK Approach**: Faster setup, automatic configuration
- **Custom FastAPI**: Full control, production flexibility

## AGENTCORE COMPLIANCE REQUIREMENTS

### Security Requirements
- ARM64 platform compliance
- Port 8080 exposure
- IAM role configuration
- Secure credential management through AgentCore Identity

### Performance Requirements
- Fast cold start optimization
- Session isolation support
- CloudWatch observability integration
- Proper error handling and logging

### Operational Requirements
- Health check endpoint implementation
- Proper request/response handling
- Container deployment readiness
- ECR integration capability

## RECOMMENDED IMPLEMENTATION SEQUENCE

### Phase 1: Foundation Setup
1. Clarify framework choice (Strands vs AgentCore native)
2. Set up Python project structure
3. Configure AgentCore SDK dependencies
4. Implement basic FastAPI structure

### Phase 2: LLM Integration
1. Choose primary LLM provider
2. Implement secure authentication pattern
3. Create basic connection testing logic
4. Add response logging and error handling

### Phase 3: AgentCore Integration
1. Configure container deployment
2. Set up ECR repository
3. Implement observability integration
4. Test local deployment with AgentCore toolkit

### Phase 4: Validation
1. Test basic LLM connectivity
2. Validate health endpoints
3. Confirm AgentCore deployment compatibility
4. Document setup and configuration

## CRITICAL GAPS IDENTIFIED

### Documentation Gap
- "AgentCore Strands" framework not found in official documentation
- Need clarification on intended framework choice
- Integration pattern depends on framework selection

### Environment Setup Gap
- No existing project structure
- Container engine requirements for local testing
- AWS credentials and ECR access requirements

### Testing Gap
- No test framework currently defined
- Need basic connectivity validation
- Health check endpoint testing required

## NEXT STEPS RECOMMENDATIONS

1. **IMMEDIATE**: Clarify "AgentCore Strands" framework intention
2. **SETUP**: Create basic Python project structure with AgentCore SDK
3. **CONFIGURE**: Set up container deployment capability
4. **IMPLEMENT**: Basic LLM connectivity with chosen provider
5. **VALIDATE**: Test agent deployment and invocation
6. **DOCUMENT**: Create setup and execution documentation

This analysis provides comprehensive guidance for AgentCore implementation while identifying critical decision points that must be resolved before proceeding with implementation.