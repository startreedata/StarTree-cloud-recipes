include ../Makefile
include ../.env

list:
	@curl ${PINOT_CONTROLLER}/schemas \
		-H "Accept: application/json" \
		-H "Authorization: Bearer ${PINOT_TOKEN}" \
		-H "Database: ${PINOT_WORKSPACE}" \
		| jq

	@curl ${PINOT_CONTROLLER}/tables \
		-H "Accept: application/json" \
		-H "Authorization: Bearer ${PINOT_TOKEN}" \
		-H "Database: ${PINOT_WORKSPACE}" \
		| jq

	@curl ${PINOT_CONTROLLER}/tables/clickstream/schema \
		-H "Accept: application/json" \
		-H "Authorization: Bearer ${PINOT_TOKEN}" \
		-H "Database: ${PINOT_WORKSPACE}" \
		| jq

	@curl ${PINOT_CONTROLLER}/tables/${PINOT_WORKSPACE}.clickstream/schema \
		-H "Accept: application/json" \
		-H "Authorization: Bearer ${PINOT_TOKEN}" \
		| jq

	@curl ${PINOT_CONTROLLER}/tables/test1 \
		-H "Accept: application/json" \
		-H "Authorization: Bearer ${PINOT_TOKEN}" \
		-H "Database: ${PINOT_WORKSPACE}" \
		| jq

table:
	@curl -X POST ${PINOT_CONTROLLER}/schemas \
		-H "Accept: application/json" \
		-H "Content-Type: application/json" \
		-H "Authorization: Bearer ${PINOT_TOKEN}" \
		-H "Database: ${PINOT_WORKSPACE}" \
		-d @config/schema.json \
		| jq

	@curl -X  POST ${PINOT_CONTROLLER}/tables \
		-H "Accept: application/json" \
		-H "Content-Type: application/json" \
		-H "Authorization: Bearer ${PINOT_TOKEN}" \
		-H "Database: ${PINOT_WORKSPACE}" \
		-d @config/table.json \
		| jq

upload:
	docker run \
		-v ${PWD}:/tmp/work/ \
		apachepinot/pinot:1.1.0 \
			LaunchDataIngestionJob \
			-jobSpecFile /tmp/work/config/ingestionJobSpec.yaml \
			-authToken " Bearer ${PINOT_TOKEN}" \
			-values PINOT_WORKSPACE=${PINOT_WORKSPACE} \
			-values PINOT_CONTROLLER=${PINOT_CONTROLLER} \
			-values DATA=/tmp/work/data/

recipe: table upload

app: helper
	docker run \
		-v ${PWD}:/tmp/work \
		-e PINOT_WORKSPACE=${PINOT_WORKSPACE} \
		-e PINOT_CONTROLLER=${PINOT_CONTROLLER} \
		-e PINOT_TOKEN=${PINOT_TOKEN} \
		-e PINOT_BROKER=${PINOT_BROKER} \
		-p 8501:8501 \
		--name app \
		startree/insterstellar \
			streamlit run /tmp/work/app.py --server.port=8501 --server.address=0.0.0.0

clean:
	docker kill app
	