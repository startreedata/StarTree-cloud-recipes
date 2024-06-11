include ../Makefile
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
	docker run \
		-v ${PWD}:/tmp/work/ \
		apachepinot/pinot:1.1.0 \
			LaunchDataIngestionJob \
			-jobSpecFile /tmp/work/config/ingestionJobSpec.yaml \
			-authToken " Bearer ${ST_TOKEN}" \
			-values ST_WORKSPACE=${ST_WORKSPACE} \
			-values ST_CONTROLLER=${ST_CONTROLLER} \
			-values DATA=/tmp/work/data/

recipe: table upload

app: helper
	docker run \
		-v ${PWD}:/tmp/work \
		-e ST_WORKSPACE=${ST_WORKSPACE} \
		-e ST_CONTROLLER=${ST_CONTROLLER} \
		-e ST_TOKEN=${ST_TOKEN} \
		-e ST_BROKER=${ST_BROKER} \
		-p 8501:8501 \
		--name app \
		startree/insterstellar \
			streamlit run /tmp/work/app.py --server.port=8501 --server.address=0.0.0.0

clean:
	docker kill app
	