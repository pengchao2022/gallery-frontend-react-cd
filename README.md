# gallery-frontend-react-cd
devops demo











# abort the canary 
kubectl argo rollouts abort frontend-react-rollout -n frontend-prod


# go back to the last stable version
kubectl argo rollouts restart frontend-react-rollout -n frontend-prod


# if it's still degrated not healthy
kubectl argo rollouts undo frontend-react-rollout -n frontend-prod

# if the app health keeps processing run this command
kubectl argo rollouts promote frontend-react-rollout -n frontend-prod

allen@maxwell ~ % kubectl argo rollouts promote frontend-react-rollout -n frontend-prod
rollout 'frontend-react-rollout' promoted
allen@maxwell ~ % kubectl argo rollouts get rollout frontend-react-rollout -n frontend-prod
Name:            frontend-react-rollout
Namespace:       frontend-prod
Status:          ◌ Progressing
Message:         waiting for rollout spec update to be observed
Strategy:        Canary
  Step:          5/5
  SetWeight:     100
  ActualWeight:  100
Images:          317429619308.dkr.ecr.us-east-1.amazonaws.com/frontend-react:3.0.0 (stable)
Replicas:
  Desired:       5
  Current:       5
  Updated:       5
  Ready:         5
  Available:     5

NAME                                                KIND        STATUS         AGE   INFO
⟳ frontend-react-rollout                            Rollout     ◌ Progressing  111m  
├──# revision:5                                                                      
│  └──⧉ frontend-react-rollout-85fc6757c7           ReplicaSet  ✔ Healthy      89m   stable
│     ├──□ frontend-react-rollout-85fc6757c7-kppl8  Pod         ✔ Running      17m   ready:1/1
│     ├──□ frontend-react-rollout-85fc6757c7-vz5j4  Pod         ✔ Running      17m   ready:1/1
│     ├──□ frontend-react-rollout-85fc6757c7-2zj29  Pod         ✔ Running      17m   ready:1/1
│     ├──□ frontend-react-rollout-85fc6757c7-vbmwn  Pod         ✔ Running      17m   ready:1/1
│     └──□ frontend-react-rollout-85fc6757c7-xcshw  Pod         ✔ Running      17m   ready:1/1
├──# revision:4                                                                      
│  └──⧉ frontend-react-rollout-7f9cfd55bc           ReplicaSet  • ScaledDown   24m   
├──# revision:2                                                                      
│  └──⧉ frontend-react-rollout-7577c4998f           ReplicaSet  • ScaledDown   101m  
└──# revision:1                                                                      
   └──⧉ frontend-react-rollout-7bf47c6544           ReplicaSet  • ScaledDown   111m  
allen@maxwell ~ % kubectl argo rollouts restart frontend-react-rollout -n frontend-prod
rollout 'frontend-react-rollout' restarts in 0s
allen@maxwell ~ % kubectl argo rollouts get rollout frontend-react-rollout -n frontend-prod
Name:            frontend-react-rollout
Namespace:       frontend-prod
Status:          ॥ Paused
Message:         CanaryPauseStep
Strategy:        Canary
  Step:          1/5
  SetWeight:     20
  ActualWeight:  20
Images:          317429619308.dkr.ecr.us-east-1.amazonaws.com/frontend-react:3.0.0 (stable)
                 317429619308.dkr.ecr.us-east-1.amazonaws.com/frontend-react:3.0.2 (canary)
Replicas:
  Desired:       5
  Current:       5
  Updated:       1
  Ready:         5
  Available:     5

NAME                                                KIND        STATUS        AGE   INFO
⟳ frontend-react-rollout                            Rollout     ॥ Paused      113m  
├──# revision:6                                                                     
│  └──⧉ frontend-react-rollout-7f9cfd55bc           ReplicaSet  ✔ Healthy     26m   canary
│     └──□ frontend-react-rollout-7f9cfd55bc-mdxrv  Pod         ✔ Running     13s   ready:1/1
├──# revision:5                                                                     
│  └──⧉ frontend-react-rollout-85fc6757c7           ReplicaSet  ✔ Healthy     90m   stable
│     ├──□ frontend-react-rollout-85fc6757c7-l5h9k  Pod         ✔ Running     12s   ready:1/1
│     ├──□ frontend-react-rollout-85fc6757c7-zxvjt  Pod         ✔ Running     11s   ready:1/1
│     ├──□ frontend-react-rollout-85fc6757c7-wfksl  Pod         ✔ Running     10s   ready:1/1
│     └──□ frontend-react-rollout-85fc6757c7-mgzxk  Pod         ✔ Running     9s    ready:1/1
├──# revision:2                                                                     
│  └──⧉ frontend-react-rollout-7577c4998f           ReplicaSet  • ScaledDown  103m  
└──# revision:1                                                                     
   └──⧉ frontend-react-rollout-7bf47c6544           ReplicaSet  • ScaledDown  113m  
allen@maxwell ~ % 

