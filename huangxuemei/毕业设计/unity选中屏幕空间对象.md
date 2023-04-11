```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Behaviour : MonoBehaviour
{
    public float smooth = 3f;
    //存储当前点击选中的对象
    Transform currentObject;
    //鼠标的位置
    Vector3 mouse3DPosition;
    void Start()
    {
        
    }
    void Update()
    {
        //鼠标点击
        if (Input.GetMouseButton(0))
        {
            //从摄像机中心到鼠标点击的位置的射线
            Ray rays = Camera.main.ScreenPointToRay(Input.mousePosition);
            //将射线以黄色的表示出来
            Debug.DrawRay(rays.origin, rays.direction * 100, Color.yellow);
            //一个RayCast变量用于存储射线的返回信息
            RaycastHit hit;
            //将创建的射线投射出去并将反馈结果存储到hit中
            if(Physics.Raycast(rays,out hit))
            {
                //获取被射线碰到的对象transform
                currentObject = hit.transform;
            }
            if (currentObject)
            {
                Debug.Log(currentObject.name);
            }
        }
    }
}
```