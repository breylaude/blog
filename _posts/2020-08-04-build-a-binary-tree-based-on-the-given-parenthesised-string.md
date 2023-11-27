---
layout: post
title: Build a binary tree based on a given parenthesized string
subtitle: 
gh-repo: breylaude/breylaude.github.io
gh-badge: [star, fork, follow]
tags: [rant]
comments: true
---

Because of debugging needs, I wrote a stack and inherited the vector implementation (if it is inherited from the general stack class, then it is a class adapter).

The code is as follows:

```c
#include<iostream>
#include<vector>
using namespace std;

struct node_t{
    chardata;
    node_t *left, *right;
    node_t(){
        data ='\0';
        left =NULL;
        right =NULL;
    }
    ~node_t() {
        delete left;
        deleteright;
    }
    friend inline ostream & operator << (ostream &os, constnode_t &t){
        os << t.data << "(" << t.left << "," << t.right << ")";
        returnos;
    }
}*root;
void print(node_t **t){
    if(*t == NULL){
        cout<< 0;
    }else {
        cout<< *t << "|" << (**t);
    }
}

void dump(node_t *t = root, int dep = 0){
    if(t != NULL){
        for(int i = 0; i <dep; ++i) printf(" ");
        print(&t);
        printf("\n");
        dump(t->left, dep+1);
        dump(t->right, dep+1);
    }
}

template<typename T>
class mystack: private vector<T>{ //rarely usedprivateInherit, hehe.
public:
    void push(constT &a){
        this->push_back(a);
    }
    voidpop(){
        this->pop_back();
    }
    T & top(){
        return*(this->rbegin());
    }
    void dump(void (*print)(T)){
        for(unsigned intj = 0; j <this->size(); ++j){
            print(this->operator[](j));
            cout<<',';
        }
        cout<< endl;
    }
};

node_t* build(char*s){
    mystack<node_t**>ooxx;
    node_t *root =newnode_t, **t;
    ooxx.push(&root);
    for(int i = 0; s[i] !='\0'; ++i){
        printf("procesing %c\n", s[i]);
        switch(s[i]){
            case '':
                break;
            case'(':
                t = ooxx.top();
                ooxx.push(&((*t)->left));
                break;
            case',':
                ooxx.pop();
                t = ooxx.top();
                ooxx.push(&((*t)->right));
                break;
            case')':
                ooxx.pop();
                break;
            default:
                t = ooxx.top();
                if(*t == NULL) *t = newnode_t;
                (*t)->data = s[i];
                break;
        }
        ooxx.dump(print);
    }
    returnroot;
}

intmain(){
    root = build("a(b(,e(f,g(h(j,),i(k,)))),c(d,))");
    dump(root);
    return0;
}
```
