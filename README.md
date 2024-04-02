# AWS Lambda Layer Tutorial
This is Tutorial repo about how to create Layer to an AWS Lambda function!

# Layer Path
You can use various libraries in your AWS Lambda function by uploading a .zip file, Which is called "Layer".

According to [Layer Path documentation](https://docs.aws.amazon.com/lambda/latest/dg/packaging-layers.html) ,

"Layer" for AWS Lambda should follow a specific folder structure:

- `Python`
```
my-layer.zip
│
└── python/
    ├── package1/
    ├── package2/
    ├── module1.py
    └── module2.py
```

- `Node.js`
```
my-layer.zip
│
└── nodejs/
    └── node_modules/
        ├── package1/
        └── package2/
```

- `Java`
```
my-layer.zip
│
└── java/
    └── lib/
        ├── library1.jar
        └── library2.jar
```

- `Ruby`
```
my-layer.zip
│
└── ruby/
    └── gems/
        ├── gem1/
        └── gem2/
```

- Custom runtime
```
my-layer.zip
│
└── bin/
    └── bootstrap
```

# Installing Libraries for the AWS Lambda Environment

According to [Documentation here](https://aws.amazon.com/premiumsupport/knowledge-center/lambda-python-package-compatible/) , AWS Lambda OS is `manylinux2014_aarch64` . 

When creating a Layer file, You should ensure that you install compatible library versions for the AWS Lambda environment. 

(Some libraries may not run on AWS Lambda when you installed them on Windows!)

To do that, In `Python`, After creating the "my-layer" folder, execute the following command to install the library:

```
pip install --platform manylinux2014_aarch64 --target=./python/lib/python3.9/site-packages --implementation cp --python 3.9 --only-binary=:all: --upgrade pandas
```

The "Example Layer.zip" in this repo contains `pandas` and `numpy`, which were installed using the command above as well ^-^! 

# Troubleshooting

Unfortunately, there are some libraries that you cannot properly install for AWS Lambda although using the command above.

In such cases, you can just use a free-tier Linux server from AWS EC2 and install the library there!

After creating a Linux instance in AWS EC2, you can create a Layer file by executing the following commands:

1. `sudo yum -y install python-pip`

2. `pip install -t ./my-layer/python --upgrade library`

Then create a .zip file by executing the following commands:

1. `sudo yum -y install zip`

2. `zip -r my-layer.zip my-layer/`

