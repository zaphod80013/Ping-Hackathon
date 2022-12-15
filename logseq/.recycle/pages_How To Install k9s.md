## From K9's official website: [https://k9scli.io/](https://k9scli.io/)

> K9s is a terminal based UI to interact with your Kubernetes clusters. The aim of this project is to make it easier to navigate, observe and manage your deployed applications in the wild.
K9s continually watches Kubernetes for changes and offers subsequent commands to interact with your observed resources.
- ![Screen_Shot_2020-10-29_at_3_04_22_PM.png](../assets/Screen_Shot_2020-10-29_at_3_04_22_PM_1671043637684_0.png)
## Step-by-step guide
- Log into your Management Node
- Run each line of the following commands:
  
  ```
  bash
  ```
- Start k9s by running:
  
  ```
  k9s
  ```
## **K9s GitHub Releases:**

**[https://github.com/derailed/k9s/releases](https://github.com/derailed/k9s/releases)**
## **Some shortcuts:**

`Ctrl-d` - Deletes the selected pods

`s` - Enter into the pod's shell

`Ctrl-c` - Quit k9 

`:` - Select object you want
- `:ns` - Lists namespaces
- `:pvc` - Lists pvc
- `:sts` - Lists stateful sets
- /