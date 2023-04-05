### 位置、旋转、变换的信息的获取
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using TMPro;

public class UIRight : MonoBehaviour
{
    private Vector3 position;
    private Vector3 rotation;
    private Vector3 scale;
    private Transform positionInput;
    private Transform rotationInput;
    private Transform scaleInput;
    public GameObject prefab;
    // Start is called before the first frame update
    void Start()
    {
        //分别获取到三个信息的输入的父节点
        positionInput = transform.GetChild(0);
        rotationInput = transform.GetChild(1);
        scaleInput = transform.GetChild(2);
        //获取到默认的三个信息
        freshTransformInfo();
    }

    // Update is called once per frame
    void Update()
    {
        
    }
    private void inputEnd(string str)
    {
        freshTransformInfo();
    }
    private void freshTransformInfo()
    {
        //分别遍历三个信息输入的父节点的子节点的输入框，对其添加更新三个信息的监听，更新三个信息
        for (int i = 0; i < 3; i++)
        {
            TMP_InputField input = positionInput.GetChild(i).GetChild(0).GetComponent<TMP_InputField>();
            input.onEndEdit.AddListener(inputEnd);
            switch (i)
            {
                case 0: position.x = float.Parse(input.text); break;
                case 1: position.y = float.Parse(input.text); break;
                case 2: position.z = float.Parse(input.text); break;
            }
        }
        for (int i = 0; i < 3; i++)
        {
            TMP_InputField input = rotationInput.GetChild(i).GetChild(0).GetComponent<TMP_InputField>();
            input.onEndEdit.AddListener(inputEnd);
            switch (i)
            {
                case 0: rotation.x = float.Parse(input.text); break;
                case 1: rotation.y = float.Parse(input.text); break;
                case 2: rotation.z = float.Parse(input.text); break;
            }
        }
        for (int i = 0; i < 3; i++)
        {
            TMP_InputField input = scaleInput.GetChild(i).GetChild(0).GetComponent<TMP_InputField>();
            input.onEndEdit.AddListener(inputEnd);
            switch (i)
            {
                case 0: scale.x = float.Parse(input.text); break;
                case 1: scale.y = float.Parse(input.text); break;
                case 2: scale.z = float.Parse(input.text); break;
            }
        }
    }
}
```
![[Pasted image 20230405170814.png]]
![[Pasted image 20230405170829.png]]
### 动态生成对象
```c#
    public void generation()
    {
        //动态生成对象
        GameObject p = GameObject.CreatePrimitive(PrimitiveType.Cube);
        Transform tran = p.GetComponent<Transform>();
        tran.position = position;
        tran.rotation = Quaternion.Euler(rotation.x, rotation.y, rotation.z);
        tran.localScale=scale;
    }
```
![[Pasted image 20230405170900.png]]