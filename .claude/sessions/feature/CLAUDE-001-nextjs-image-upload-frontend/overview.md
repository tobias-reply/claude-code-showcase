# Session Overview: Next.js Image Upload Frontend

**Branch**: `feature/CLAUDE-001-nextjs-image-upload-frontend`
**Ticket**: #CLAUDE-001 - Develop Next.js frontend with PNG image upload for accessibility tool

## Session Progress

- [X] Presented all experts to user and decided which experts to consult in what order
- [X] Branch created
- [X] Session directory created
- [X] Adjusted Session Progress based on the experts that need to be consulted
- [X] Initial overview from `.claude/sessions/example/overview.md` copied to directory
- [X] Expert consultations
   - [X] Security Expert consultation
   - [X] Transcribe security analysis to overview.md
- [ ] Create comprehensive implementation plan in overview.md based on expert consultation
- [ ] Make sure that user is happy with the plan presented in overview.md
- [ ] Implementation of overview.md with user
- [ ] Testing and validation
- [ ] Review sub-agents based on branch changes
   - [ ] Ask the user if new sub-agents should be built in .claude/agents based on .claude/agents/expert_template.md
   - [ ] Edit the knowledge of existing agents in .claude/agents
- [ ] Update readmes

## Brief Summary

Develop an accessibility-focused Next.js frontend interface with PNG image upload capabilities for a tool that helps visually impaired users generate image descriptions for blog posts. The interface must be WCAG 2.1 AA compliant with drag-and-drop functionality, proper error handling, and screen reader compatibility.

### Requirements/Tasks
1. **Next.js Application Setup**
   - Modern React patterns and component structure
   - Responsive design optimized for all screen sizes
   - Build optimization for production deployment

2. **PNG File Upload Interface**
   - Drag-and-drop upload area with visual indicators
   - Click-to-upload button functionality
   - Client-side PNG validation (reject other formats)
   - File size validation (max 10MB)
   - Loading states with descriptive text
   - Success states showing generated descriptions

3. **Accessibility Requirements (WCAG 2.1 AA)**
   - Screen reader compatibility with proper ARIA labels
   - Keyboard navigation support
   - High contrast mode compatibility
   - Proper heading structure and semantic HTML
   - Focus management during upload process
   - Screen reader announcements for upload states

4. **Error Handling & Edge Cases**
   - Clear error messages for non-PNG files
   - File size limit exceeded handling
   - Network error handling during API calls
   - Timeout handling for slow connections
   - Multiple file upload prevention/handling

## Acceptance Criteria

- [ ] User can drag and drop PNG files onto designated upload area
- [ ] User can click upload button to select PNG files from file system
- [ ] System clearly rejects non-PNG files with helpful error message
- [ ] Upload area provides clear visual feedback during drag operations
- [ ] Loading states are accessible to screen readers
- [ ] Generated descriptions are displayed in accessible format
- [ ] Interface works on desktop and mobile devices
- [ ] All accessibility standards met for visually impaired users

## When Closing (Required)

- [ ] Screenshots of working upload interface
- [ ] Video demo showing drag-drop and button upload functionality
- [ ] Accessibility testing results (screen reader compatibility)
- [ ] Error handling demonstrations (wrong file type, oversized files)
- [ ] Responsive design verification across devices
- [ ] Link to frontend code repository and PR

## Implementation Approach

Next.js 14+ with App Router, TypeScript for type safety, Tailwind CSS for responsive styling, and React hooks for state management. No authentication required - simple standalone upload interface with future API integration readiness.

## Expert Consultation Plan

Based on the requirements, we need consultation from:

1. **Security Expert** - Primary consultation needed for:
   - File upload security best practices
   - Client-side validation security considerations
   - Input sanitization and XSS prevention
   - File size and type validation security
   - Accessibility security considerations (WCAG compliance)
   - Error message security (avoiding information disclosure)

*Note: AgentCore expert not needed - this is a standalone frontend not connected to AgentCore systems*

## Key Findings

### Security Expert - CRITICAL SECURITY REQUIREMENTS

**Risk Level: HIGH** - File upload functionality represents a critical attack surface.

#### 1. File Upload Security (CRITICAL)
- **Server-side validation is MANDATORY** - Never trust client-side validation alone
- Validate PNG magic bytes: `89 50 4E 47 0D 0A 1A 0A` on server
- Enforce 10MB file size limit on both client and server
- Use random UUID filenames (never use user-provided filenames directly)
- Implement strict Content-Type validation with magic byte verification
- Use secure image processing library (sharp, not imagemagick)
- Strip metadata (EXIF, IPTC, XMP) from uploaded images

#### 2. XSS Prevention (HIGH)
- Rely on React's automatic escaping (never use dangerouslySetInnerHTML for user content)
- Sanitize filenames before display using DOMPurify if needed
- Implement comprehensive Content Security Policy (CSP)
- Use Next.js Image component with domain restrictions
- Generic error messages only (no user input in error messages)

#### 3. CSRF Protection (CRITICAL)
- Implement CSRF tokens for all upload requests
- Validate Origin header against whitelist
- Configure SameSite cookie attribute to 'strict'
- Proper CORS configuration (same-origin only)

#### 4. DoS Protection (CRITICAL)
- Rate limiting: max 10 uploads per 15 minutes per IP
- Request timeout: 30 seconds maximum
- Stream processing for large files (avoid loading entire file into memory)
- Temporary file cleanup after processing

#### 5. Accessibility Security (MEDIUM)
- ARIA labels must not expose sensitive information (file paths, server details)
- Generic error messages for screen readers
- Secure keyboard event handling (validate focus targets)

#### 6. Environment Security (CRITICAL)
- All secrets in .env files (.gitignore)
- HTTPS enforcement in production
- Comprehensive security headers (CSP, X-Frame-Options, HSTS, X-Content-Type-Options)
- Disable source maps in production

#### 7. Dependency Security (HIGH)
- Regular npm audit before deployment
- Automated Dependabot for security updates
- Use npm ci in production (not npm install)
- CI/CD security scanning

#### 8. Required Security Testing
- Manual file upload attack vectors testing
- XSS testing (malicious filenames, error messages)
- CSRF testing (missing/invalid tokens)
- DoS testing (concurrent uploads, timeouts)
- Automated OWASP ZAP/Burp Suite scanning
- Accessibility testing with screen readers

## Implementation Decisions

### Technology Stack
- **Framework**: Next.js 14+ with App Router
- **Language**: TypeScript for type safety
- **Styling**: Tailwind CSS for responsive design
- **Image Processing**: sharp library (secure, fast)
- **Security Libraries**:
  - next-csrf for CSRF protection
  - isomorphic-dompurify for sanitization
  - express-rate-limit for rate limiting
  - helmet for security headers

### Security Architecture
1. **Dual Validation Strategy**:
   - Client-side: User experience (early feedback)
   - Server-side: Security enforcement (mandatory)

2. **File Handling**:
   - Random UUID filenames only
   - Store original filename in database/state only
   - Magic byte verification on server
   - Metadata stripping before storage

3. **API Protection**:
   - CSRF tokens for all mutations
   - Rate limiting per IP address
   - Origin validation
   - Generic error responses

### Deployment Requirements
- HTTPS only in production
- Environment variable validation on startup
- Security headers configured in next.config.js
- Production build without source maps
- Automated security scanning in CI/CD

