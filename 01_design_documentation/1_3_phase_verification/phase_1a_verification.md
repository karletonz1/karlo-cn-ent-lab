# Phase 2 Verification

## Ansible 'Deploy North Star' Playbook for Phase 2  

![phase_2_deploy_part_1](../../Assets/Images/phase_2_deploy_part_1.png)
![phase_2_deploy_part_2](../../Assets/Images/phase_2_deploy_part_2.png)
![phase_2_deploy_part_3](../../Assets/Images/phase_2_deploy_part_3.png)

## MLAG between Spine-01 and Spine-02  

![mlag_spine_01](../../Assets/Images/mlag_spine_01.png) ![mlag_spine_02](../../Assets/Images/mlag_spine_02.png)

## Leaf LACP Peer  

**Leaf 1**  

![lacp_leaf_01](../../Assets/Images/lacp_leaf_01.png)

**Leaf 2**  

![lacp_leaf_02](../../Assets/Images/lacp_leaf_02.png)

> [!NOTE]  
> By subtracting 32768 from the number found in the Port# column, we can verify the correct secondary port is in use

## Spanning Tree  

**Leaf 1**  
![mstp_leaf_01](../../Assets/Images/mstp_leaf_01.png)

**Leaf 2**  
![mstp_leaf_02](../../Assets/Images/mstp_leaf_02.png)

**Spine 1**  
![mstp_spine_01](../../Assets/Images/mstp_spine_01.png)

**Spine 2**  
![mstp_spine_02](../../Assets/Images/mstp_spine_02.png)
