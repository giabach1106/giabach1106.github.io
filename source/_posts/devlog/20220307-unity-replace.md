---
title: UnityPrefab批量替换GameObject脚本
date: 2022-03-07
categories: Devlog
tags: 
- 游戏开发
- 关卡设计
cover: /images/20220307-replace.jpg
toc: true
---
问题解决了，但轮子是别人造的。“我和科比合砍81分”。

<!--more-->

---

## 遭遇问题

Unity2018版本之前，将一个Prefab的instance载入另一个Prefab下并Apply，会导致原本Prefab的连接断开。

<br/>

## 期望目标

保持原有Prefab的链接关系，同时还要把它们做成一个Prefab来给工程用。

在这里学到了Unity多场景编辑的妙用：将想编辑的东西单独在另一个Scene里编辑，这个Scene里会保持所有Prefab的链接关系。当需要做成一个Prefab时，再把内容Copy过去。

*当然，如果不是Unity版本太老，根本不会产生这个问题和这套拐弯解决的方案。*

<br/>

## 实操过程

知道要做什么了，却没有马上开始做。因为当前的Prefab已经全部断开了链接关系，没法直接使用上面的方法。

最简单粗暴的方法就是，新建个空Scene然后**全部重新手摆一遍**。我无法接受。

其次想到的是，Unity是否有UE4的replace功能？查了下*果然*没有。然后在Google上搜索“Unity Replace gameobject with prefab”相关字眼，最后找到Unity forum一条从2009年更新至今的讨论串。*原来这个破引擎十年了还没做出来个replace功能是吗*

Unity工程下的Editor里可以放那种脚本，当作插件来使用，非常便利。找到了讨论串最新的脚本，复制贴贴进来打算开用，然后compile报错，说找不到某个函数定义。又Google了下Unity的API，发现这个函数最早是2018版才更新进去的。*2017到底是有多老*

退而求其次，往回回溯版本，注释掉了会引起问题的方法，保留了可运行部分。毕竟我只是需要一个replace功能，根据tag和layer等分类的功能还用不到。

下面为2017可运行的脚本，保存一份留档。

```
using UnityEngine;
using UnityEditor;
using UnityEditor.SceneManagement;
using System.Collections.Generic;
 
// COMMUNITY THREAD LINK https://forum.unity.com/threads/replace-game-object-with-prefab.24311/
// CopyComponents - by Michael L. Croswell for Colorado Game Coders, LLC
// March 2010
//Modified by Kristian Helle Jespersen
//June 2011
//Modified by Connor Cadellin McKee for Excamedia
//April 2015
//Modified by Fernando Medina (fermmmm)
//April 2015
//Modified by Julien Tonsuso (www.julientonsuso.com)
//July 2015
//Changed into editor window and added instant preview in scene view
//Modified by Alex Dovgodko
//June 2017
//Made changes to make things work with Unity 5.6.1
//March 2018
//Added link to community thread, booleans to chose if scale and rotation are applied, mark scene as dirty, changed menu item to tools. By Hyper
//May 2018
//Added KeepPlaceInHeirarchy self explanatory
//Modified by Virgil Iordan
public class ReplaceWithPrefab:EditorWindow {
    public GameObject Prefab;
    public GameObject[] ObjectsToReplace;
    public List<GameObject> TempObjects = new List<GameObject>();
    public bool KeepOriginalNames = true;
    public bool EditMode = false;
    public bool ApplyRotation = true;
    public bool ApplyScale = true;
    public bool KeepPlaceInHeirarchy = false;
    // Add menu named "My Window" to the Window menu
    [MenuItem("Tools/ReplaceWithPrefab")]
    static void Init() {
        // Get existing open window or if none, make a new one:
        ReplaceWithPrefab window = (ReplaceWithPrefab)EditorWindow.GetWindow(typeof(ReplaceWithPrefab));
        window.Show();
    }
    void OnSelectionChange() {
        GetSelection();
        Repaint();
    }
 
    void CheckParent() {
    }
 
    void OnGUI() {
        EditMode = GUILayout.Toggle(EditMode,"Edit");
        if (GUI.changed) {
            if (EditMode)
                GetSelection();
            else
                ResetPreview();
        }
        KeepOriginalNames = GUILayout.Toggle(KeepOriginalNames,"Keep names");
        ApplyRotation = GUILayout.Toggle(ApplyRotation,"Apply rotation");
        ApplyScale = GUILayout.Toggle(ApplyScale,"Apply scale");
        KeepPlaceInHeirarchy = GUILayout.Toggle(KeepPlaceInHeirarchy,"Keep Place In Heirarchy");
        GUILayout.Space(5);
        if (EditMode) {
            ResetPreview();
 
            GUI.color = Color.yellow;
            if (Prefab != null) {
                GUILayout.Label("Prefab: ");
                GUILayout.Label(Prefab.name);
            } else {
                GUILayout.Label("No prefab selected");
            }
            GUI.color = Color.white;
 
            GUILayout.Space(5);
            GUILayout.BeginScrollView(new Vector2());
            foreach (GameObject go in ObjectsToReplace) {
                GUILayout.Label(go.name);
                if (Prefab != null) {
                    GameObject newObject;
                    newObject = (GameObject)PrefabUtility.InstantiatePrefab(Prefab);
                    newObject.transform.SetParent(go.transform.parent,true);
                    newObject.transform.localPosition = go.transform.localPosition;
                    if (ApplyRotation) {
                        newObject.transform.localRotation = go.transform.localRotation;
                    }
                    if (ApplyScale) {
                        newObject.transform.localScale = go.transform.localScale;
                    }
                    TempObjects.Add(newObject);
                    if (KeepOriginalNames) {
                        newObject.transform.name = go.transform.name;
                    }
                    go.SetActive(false);
                    if (KeepPlaceInHeirarchy) {
                        newObject.transform.SetSiblingIndex(go.transform.GetSiblingIndex());
                    }
                }
            }
            GUILayout.EndScrollView();
            GUILayout.Space(5);
            GUILayout.BeginHorizontal();
            if (GUILayout.Button("Apply")) {
                foreach (GameObject go in ObjectsToReplace) {
                    DestroyImmediate(go);
                }
                EditMode = false;
                EditorSceneManager.MarkSceneDirty(EditorSceneManager.GetActiveScene()); // So that we don't forget to save...
            };
            if (GUILayout.Button("Cancel")) {
                ResetPreview();
                EditMode = false;
            };
            GUILayout.EndHorizontal();
        } else {
            ObjectsToReplace = new GameObject[0];
            TempObjects.Clear();
            Prefab = null;
        }
 
    }
    void OnDestroy() {
        ResetPreview();
    }
    void GetSelection() {
        if (EditMode && Selection.activeGameObject != null) {
            PrefabType t = PrefabUtility.GetPrefabType(Selection.activeGameObject);
            if (t == PrefabType.Prefab) //Here goes the fix
            {
                Prefab = Selection.activeGameObject;
            } else {
                ResetPreview();
                ObjectsToReplace = Selection.gameObjects;
            }
        }
    }
    void ResetPreview() {
        if (TempObjects != null) {
            foreach (GameObject go in TempObjects) {
                DestroyImmediate(go);
            }
        }
        foreach (GameObject go in ObjectsToReplace) {
            go.SetActive(true);
        }
        TempObjects.Clear();
    }
}

```

扔进Editor就可以用了。这个东西比想象中好用，选取任意数量的gameobject，然后点选Project视图里的prefab，就能一键全部替换，保留原本的transom和hierarchy位置。除去干上面的脏活儿，以后做关卡时也可以经常用。

*千万记得别上传，不想被QA查水表*
