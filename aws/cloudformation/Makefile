
ENV=qa
STACK_NAME=pm-av-$(ENV)
PROFILE=default #use for prod
GLOBAL_STACK_NAME=pm-av-$(ENV)
REGION=us-east-1

upload-env:
	@aws s3api create-bucket --bucket pm-templates-us-east-1-$(ENV) --region us-east-1
	@aws s3 cp _main.yml s3://pm-templates-us-east-1-$(ENV)/cloudformation/_main.yml
	@aws s3 cp resources/iam.yml s3://pm-templates-us-east-1-$(ENV)/cloudformation/$(ENV)/iam.yml
	@aws s3 cp resources/ecs.yml s3://pm-templates-us-east-1-$(ENV)/cloudformation/$(ENV)/ecs.yml
	@aws s3 cp resources/rds.yml s3://pm-templates-us-east-1-$(ENV)/cloudformation/$(ENV)/rds.yml


create-env:
	@aws cloudformation create-stack \
		--region $(REGION) \
		--stack-name $(STACK_NAME) \
		--template-url https://s3.amazonaws.com/pm-templates-us-east-1-$(ENV)/cloudformation/_main.yml \
		--parameters file://parameters.$(ENV).json \
		--capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
		--profile $(PROFILE) \
		--tags file://tagging.json

update-env:
	@aws cloudformation update-stack \
		--region $(REGION) \
		--stack-name $(STACK_NAME) \
		--template-url https://s3.amazonaws.com/pm-templates-us-east-1-$(ENV)/cloudformation/_main.yml \
		--parameters file://parameters.$(ENV).json \
		--capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND \
		--profile $(PROFILE) \
		--tags file://tagging.json

delete-env:
	@aws cloudformation delete-stack \
		--region $(REGION) \
		--stack-name $(STACK_NAME) \
		--profile $(PROFILE)

validate-env:
	@aws cloudformation validate-template \
		--template-url https://s3.amazonaws.com/pm-templates-us-east-1-$(ENV)/cloudformation/_main.yml
	@aws cloudformation validate-template \
		--template-url https://s3.amazonaws.com/pm-templates-us-east-1-$(ENV)/cloudformation/$(ENV)/ecs.yml
	@aws cloudformation validate-template \
		--template-url https://s3.amazonaws.com/pm-templates-us-east-1-$(ENV)/cloudformation/$(ENV)/rds.yml

## Not applicable for nested stacks - at the time of writing
create-changeset:
	@aws cloudformation create-change-set \
		--stack-name $(STACK_NAME) \
		--change-set-type UPDATE \
		--template-url https://s3.amazonaws.com/pm-templates-us-east-1-$(ENV)/cloudformation/_main.yml \
		--parameters file://parameters.$(ENV).json \
		--change-set-name 'a1234'