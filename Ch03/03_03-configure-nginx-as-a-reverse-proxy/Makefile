NAME = start-app-servers
FMT  = black --check --diff *.py || true
LINT = flake8 --max-line-length=88 --ignore=E203,W503
RUN  = python3 start_app_servers.py

# Colors for output
GREEN = \033[0;32m
YELLOW = \033[0;33m
NC = \033[0m # No Color

all: development fmt lint
	@echo "$(GREEN)All checks passed!$(NC)"

development:
	@echo "$(GREEN)Installing development dependencies...$(NC)"
	@pip install --requirement requirements.txt > /dev/null 2>&1
	@echo "$(GREEN)Development dependencies installed$(NC)"

fmt:
	@echo "$(GREEN)Running formatter...$(NC)"
	@$(FMT)

lint:
	@echo "$(GREEN)Running linter...$(NC)"
	@$(LINT)

run:
	@echo "$(GREEN)Starting HTTP servers...$(NC)"
	@$(RUN)

clean:
	@echo "$(YELLOW)Cleaning generated files...$(NC)"
	@find . -type d -name "__pycache__" -exec rm -r {} + 2>/dev/null || true
	@find . -type f -name "*.pyc" -delete 2>/dev/null || true
	@find . -type f -name "*.pyo" -delete 2>/dev/null || true
	@find . -type f -name ".mypy_cache" -delete 2>/dev/null || true
	@find . -type d -name ".pytest_cache" -exec rm -r {} + 2>/dev/null || true
	@find . -type f -name "junit.xml" -delete 2>/dev/null || true
	@find . -type f -name ".coverage" -delete 2>/dev/null || true
	@find . -type d -name "htmlcov" -exec rm -r {} + 2>/dev/null || true
	@echo "$(GREEN)Clean complete$(NC)"

.PHONY: development fmt lint run clean all
