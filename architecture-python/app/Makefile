unit-test:
	pytest --cov=rentomatic --cov-report=term-missing

integration-test:
	pytest -m integration

run-cli:
	./cli.py

run-dev:
	FLASK_CONFIG="development" flask run