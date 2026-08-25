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


1. Refresh（刷新 / 重新检查）
核心作用：让 Argo CD 重新去检查你的 Git 仓库和 Kubernetes 集群当前的实际状态，看看有没有发生变化。

它做什么：

它不会把任何配置应用到集群中。

它会去拉取 Git 仓库最新的 Commit，并重新运行 Helm / Kustomize（如果你的应用配置了它们）来计算出最新的期望状态（Desired State）。

同时，它会重新查询 Kubernetes 集群中资源的真实状态（Actual State），用来更新 Argo CD 界面上的 Diff（差异对比）。

什么时候用：

你刚向 Git 仓库 Push 了代码，但 Argo CD 界面上还没有显示出变化，你想让它立刻去检查。

你在集群里通过其他方式（比如 kubectl）手动修改了某些东西，你想让 Argo CD 尽快发现这些差异。

2. Sync（同步 / 部署）
核心作用：把 Git 仓库里的配置（期望状态）真正应用（Apply）到 Kubernetes 集群中去。

它做什么：

它会对比当前的期望状态（Git）和集群的实际状态（Live State）。

如果发现不一致，它就会执行 kubectl apply，把 Git 里的最新变更落地到集群里（比如创建新 Pod、更新镜像标签、修改 ConfigMap 等）。

什么时候用：

当你的应用设置了 Prune / Auto-Sync 关闭（手动同步模式），Git 仓库有更新后，你需要点击 Sync 来触发实际的上线部署。

当某次部署卡住或失败，你修正了问题后，需要手动触发 Sync 来重新部署。

💡 一个形象的比喻
Refresh 像是“对账”：会计拿着账本（Git）和仓库里的货（Kubernetes）对一遍，看看账目和实物是不是对得上，不发生货物的搬动。

Sync 像是“进货/发货”：根据对好的账单差异，真正把货搬进仓库或者从仓库搬走，产生实际的集群变更动作。