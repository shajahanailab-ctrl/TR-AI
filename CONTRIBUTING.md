# Contributing Guide

## Code of Conduct

Please treat all community members with respect and kindness. Report any violations to the project maintainers.

## Getting Started

1. Fork the repository
2. Clone your fork: `git clone https://github.com/YOUR_USERNAME/buildsmart-ai.git`
3. Create a branch: `git checkout -b feature/your-feature`
4. Make changes following our code standards
5. Test your changes thoroughly
6. Commit with clear messages
7. Push to your fork
8. Create a Pull Request

## Development Standards

### Code Style

**Python (Backend):**
- Follow PEP 8
- Use type hints for all functions
- Maximum line length: 100 characters
- Use meaningful variable names

```python
def predict_cost(
    construction_area: float,
    building_type: str,
    city: str
) -> Dict[str, float]:
    """Predict construction cost with parameters.
    
    Args:
        construction_area: Area in square feet
        building_type: Type of building
        city: City name
        
    Returns:
        Dictionary with cost breakdown
    """
    pass
```

**TypeScript/React (Frontend):**
- Use TypeScript strict mode
- Use functional components with hooks
- Use const for components and functions
- Add JSDoc comments for complex logic

```typescript
/**
 * Cost estimator component
 * @param initialCost - Initial cost estimate
 * @returns Rendered cost calculator
 */
export const CostEstimator: React.FC<{initialCost: number}> = ({
  initialCost
}) => {
  // Implementation
}
```

### Commit Messages

Use clear, descriptive commit messages:

```
feat: Add cost prediction API endpoint
fix: Correct timeline calculation for villa buildings
docs: Update API documentation
refactor: Simplify risk calculation logic
test: Add unit tests for design service
```

Format: `<type>: <subject>`

Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`

### Pull Request Process

1. Update documentation if needed
2. Add tests for new functionality
3. Run all tests locally
4. Ensure CI/CD passes
5. Request review from maintainers
6. Address review feedback
7. Keep commits clean and organized

## Testing Requirements

### Backend Tests

- Minimum 80% code coverage
- All public APIs must have tests
- Test both happy path and error cases
- Use fixtures for test data

```python
def test_cost_estimation_residential():
    """Test cost estimation for residential building"""
    result = CostService.estimate_cost(
        building_type="residential",
        construction_area=3000,
        city="New York"
    )
    
    assert result["estimated_cost"] > 0
    assert result["confidence_score"] > 0.8
```

### Frontend Tests

- Test component rendering
- Test user interactions
- Test API integration
- Minimum 70% code coverage

```typescript
describe('CostEstimator Component', () => {
  test('renders input fields', () => {
    render(<CostEstimator initialCost={100000} />)
    expect(screen.getByLabelText('Construction Area')).toBeInTheDocument()
  })
  
  test('calculates total cost on input change', async () => {
    render(<CostEstimator initialCost={100000} />)
    const input = screen.getByLabelText('Multiplier')
    fireEvent.change(input, { target: { value: '2' } })
    
    expect(screen.getByText('200000')).toBeInTheDocument()
  })
})
```

## Documentation Standards

- Keep README.md up to date
- Document public APIs with examples
- Add comments for complex logic
- Include docstrings for all functions
- Update CHANGELOG.md for releases

## Feature Development Checklist

- [ ] Feature branch created
- [ ] Code written following standards
- [ ] Unit tests added (80%+ coverage)
- [ ] Integration tests added
- [ ] Documentation updated
- [ ] README examples added
- [ ] No breaking changes OR documented migration path
- [ ] Peer review completed
- [ ] CI/CD passes
- [ ] Merge conflict resolved

## Bug Report Template

```markdown
## Description
Brief description of the bug

## Steps to Reproduce
1. ...
2. ...
3. ...

## Expected Behavior
What should happen

## Actual Behavior
What actually happens

## Environment
- OS: [e.g., Windows, macOS, Linux]
- Python: [e.g., 3.12]
- Node: [e.g., 20.0]

## Screenshots
[If applicable]

## Additional Context
[Any other relevant information]
```

## Feature Request Template

```markdown
## Description
Clear description of what you need

## Use Case
Why do you need this feature?

## Proposed Solution
How should it work?

## Alternatives Considered
Other approaches you've thought of

## Additional Context
[Any other relevant information]
```

## Code Review Guidelines

### For Authors
- Make small, focused PRs (< 500 lines)
- Write clear PR descriptions
- Address all feedback promptly
- Keep discussion professional

### For Reviewers
- Review for correctness, not perfection
- Provide actionable feedback
- Suggest improvements
- Be respectful and constructive

## Performance Considerations

When contributing, consider:
- Query efficiency (use indexes, avoid N+1)
- Algorithm complexity (aim for O(n) or better)
- Memory usage
- Bundle size (frontend)
- Rendering performance

## Security Best Practices

- Never commit secrets or API keys
- Validate all user input
- Use parameterized queries
- Hash passwords with bcrypt
- Follow OWASP guidelines
- Report security issues privately

## Release Process

1. Update version in package.json and backend/__init__.py
2. Update CHANGELOG.md
3. Create release branch: `release/v1.2.0`
4. Run full test suite
5. Build Docker images
6. Tag release: `git tag v1.2.0`
7. Push tag to trigger release
8. Create GitHub release with changelog

## Project Structure

```
buildsmart-ai/
├── backend/                 # FastAPI application
│   ├── app/
│   │   ├── api/            # API routes
│   │   ├── models/         # Database models
│   │   ├── services/       # Business logic
│   │   ├── core/           # Configuration, security
│   │   └── db/             # Database setup
│   ├── tests/              # Test files
│   └── requirements.txt
├── frontend/               # Next.js application
│   ├── app/                # Pages and components
│   ├── lib/                # Utilities
│   ├── components/         # Reusable components
│   └── package.json
├── devops/                 # Deployment configs
└── ml-models/              # ML training scripts
```

## Communication Channels

- **Issues**: GitHub Issues for bugs and feature requests
- **Discussions**: GitHub Discussions for ideas and questions
- **Email**: buildsmart-dev@example.com for security issues
- **Discord**: Join our community server

## Additional Resources

- [Python Style Guide (PEP 8)](https://pep8.org/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [React Documentation](https://react.dev/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Git Best Practices](https://git-scm.com/book/en/v2)

## License

By contributing to this project, you agree that your contributions will be licensed under the MIT License.

Thank you for contributing to BuildSmart AI! 🚀
