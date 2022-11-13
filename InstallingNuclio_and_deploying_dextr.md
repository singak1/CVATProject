### Instructions to install Openvino>Nuclio & deploying Dextr
##### Note: These instructions were intended to be used for Windows 10 or above users running CVAT locally using WSL backend running Ubuntu

1. You need to compose a serverless container in you docker:
To do that please go to your root cvat directory in bash and enter the following cmd:

          `docker-compose -f docker-compose.yml -f components/serverless/docker-compose.serverless.yml up -d`

2. You will need to install the `nuctl` CLI, this helps you build and deploy functions(dextr in our case). Please Download version [1.8.14.](https://github.com/nuclio/nuclio/releases/tag/1.8.14)
Make sure to download the `nuctl-1.8.14-linux-amd64` version if your running windows 10 with WSL backend.

      Make sure to download the nuctl file to your root cvat/ directory otherwise creating links has been giving some people issues. Once ensured the file is in the cvat/
directory we need to give it a proper permission and do a softlink :

        sudo chmod +x nuctl-<version>-linux-amd64
        sudo ln -sf $(pwd)/nuctl-<version>-linux-amd64 /usr/local/bin/nuctl
       
3. Create cvat project inside nuclio dashboard
      `nuctl create project cvat`
      This will create a cvat project within Nuclio and finally to deploy dextr enter the following cmd:
      ```
      nuctl deploy --project-name cvat \
      --path serverless/openvino/dextr/nuclio \
      --volume `pwd`/serverless/common:/opt/nuclio/common \
      --platform local
      ```
      
      ###### Known issue, Windows EOL in the python3 script: If you run into errors after trying to deploy dextr along the lines of `failed to deploy function...`, use this to resolve it
      
      run: `file serverless/common/openvino/python3` ----> If the return is something along the lines of `serverless/common/openvino/python3: Bourne-Again shell script, ASCII text executable, with CRLF line terminators`
      , that means you have windows line-endings in the file. Fix this by running
      
      ` dos2unix serverless/common/openvino/python3`  you might need to install dos2unix beforehand, using `sudo apt install dos2unix`
      
      this should return something along the lines of   `dos2unix: converting file serverless/common/openvino/python3 to Unix format...`
      
      once ran, you can now run the nuctl deploy cmd again and it should run without the errors now.
      
#### Once done go to localhost:8070 and under cvat you should see dextr running, see below image
![image](https://user-images.githubusercontent.com/112450716/201499620-7f6e3236-3ca7-46f1-baed-81effccf884f.png)
