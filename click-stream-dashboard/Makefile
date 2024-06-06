include ../.env

list:
	@curl ${ST_CONTROLLER}/schemas \
		-H "Accept: application/json" \
		-H "Authorization: Bearer ${ST_TOKEN}" \
		-H "Database: ${ST_WORKSPACE}" \
		| jq

	@curl ${ST_CONTROLLER}/tables \
		-H "Accept: application/json" \
		-H "Authorization: Bearer ${ST_TOKEN}" \
		-H "Database: ${ST_WORKSPACE}" \
		| jq

	@curl ${ST_CONTROLLER}/tables/clickstream/schema \
		-H "Accept: application/json" \
		-H "Authorization: Bearer ${ST_TOKEN}" \
		-H "Database: ${ST_WORKSPACE}" \
		| jq

	@curl ${ST_CONTROLLER}/tables/${ST_WORKSPACE}.clickstream/schema \
		-H "Accept: application/json" \
		-H "Authorization: Bearer ${ST_TOKEN}" \
		| jq

table:
	@curl -X POST ${ST_CONTROLLER}/schemas \
		-H "Accept: application/json" \
		-H "Content-Type: application/json" \
		-H "Authorization: Bearer ${ST_TOKEN}" \
		-H "Database: ${ST_WORKSPACE}" \
		-d @config/schema.json \
		| jq

	@curl -X  POST ${ST_CONTROLLER}/tables \
		-H "Accept: application/json" \
		-H "Content-Type: application/json" \
		-H "Authorization: Bearer ${ST_TOKEN}" \
		-H "Database: ${ST_WORKSPACE}" \
		-d @config/table.json \
		| jq

upload:
	pinot-admin \
		LaunchDataIngestionJob \
		-jobSpecFile ${PWD}/config/ingestionJobSpec.yaml \
		-authToken " Bearer ${ST_TOKEN}" \
		-values ST_WORKSPACE=${ST_WORKSPACE} \
		-values ST_CONTROLLER=${ST_CONTROLLER} \
		-values PWD=${PWD}

recipe: table upload

clean:
	rm -rf segments/*
