---
layout: post
title:  Android��ʾ���ؿ�
description: ���ؿ��Ǻܳ��õ�,�����ǲ�֪��д�ĸ�,�����öԻ�������ʾ,�ܲ���,����صؼ�¼��һ���ȽϺ��ʵļ��ؿ�
keyword: android,progressDialog
---

#Android���ؿ�--android.app.ProgressDialog
				
				
				
*����Ŀ���淢�ֵ�,����Ϊ��д��ϸ�Ĳ���,������������һƪ����,ת������
*ԭ����[ProgressDialogʹ���ܽ�](http://blog.csdn.net/caesardadi/article/details/11982721#)				
##ProgressDialog��ʹ�� 
###ProgressDialog �̳���AlertDialog��AlertDialog�̳���Dialog,ʵ��DialogInterface�ӿڡ�

###ProgressDialog�Ĵ�����ʽ�����֣�һ����new Dialog ,һ���ǵ���Dialog�ľ�̬����Dialog.show()��		

####��ʽһ��new Dialog  
    final ProgressDialog dialog = new ProgressDialog(this);  
    dialog.show(); 
	
####��ʽ����ʹ�þ�̬��ʽ��������ʾ�����ֽ�����ֻ����Բ����,����title��Message��ʾ����  
    ProgressDialog dialog2 = ProgressDialog.show(this, "��ʾ", "���ڵ�½��");  
	
####��ʽ�� ʹ�þ�̬��ʽ��������ʾ�����ֽ�����ֻ����Բ����,�������һ������boolean indeterminate�����Ƿ��ǲ���ȷ��״̬  
    ProgressDialog dialog3 = ProgressDialog  
            .show(this, "��ʾ", "���ڵ�½��", false);  
			
####��ʽ�� ʹ�þ�̬��ʽ��������ʾ�����ֽ�����ֻ����Բ����,�������һ������boolean cancelable �����Ƿ�������ǿ���ȡ����  
    ProgressDialog dialog4 = ProgressDialog.show(this, "��ʾ", "���ڵ�½��",  
            false, true); 
			
####��ʽ�� ʹ�þ�̬��ʽ��������ʾ�����ֽ�����ֻ����Բ����,�������һ������ DialogInterface.OnCancelListener  
####cancelListener���ڼ�����������ȡ��  
    ProgressDialog dialog5 = ProgressDialog.show(this, "��ʾ", "���ڵ�½��", true,  
            true, cancelListener);  
			
			
##ProgressDialog����ʽ�����֣�һ����Բ�β���ȷ״̬��һ����ˮƽ������״̬

###��һ�ַ�ʽ��Բ�ν�����
	final ProgressDialog dialog = new ProgressDialog(this);  
        dialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);// ���ý���������ʽΪԲ��ת���Ľ�����  
        dialog.setCancelable(true);// �����Ƿ����ͨ�����Back��ȡ��  
        dialog.setCanceledOnTouchOutside(false);// �����ڵ��Dialog���Ƿ�ȡ��Dialog������  
        dialog.setIcon(R.drawable.ic_launcher);//  
        // ������ʾ��title��ͼ�꣬Ĭ����û�еģ����û������title�Ļ�ֻ����Icon�ǲ�����ʾͼ���  
        dialog.setTitle("��ʾ");  
        // dismiss����  
        dialog.setOnDismissListener(new DialogInterface.OnDismissListener() {  
  
            @Override  
            public void onDismiss(DialogInterface dialog) {  
                // TODO Auto-generated method stub  
  
            }  
        });  
        // ����Key�¼������ݸ�dialog  
        dialog.setOnKeyListener(new DialogInterface.OnKeyListener() {  
  
            @Override  
            public boolean onKey(DialogInterface dialog, int keyCode,  
                    KeyEvent event) {  
                // TODO Auto-generated method stub  
                return false;  
            }  
        });  
        // ����cancel�¼�  
        dialog.setOnCancelListener(new DialogInterface.OnCancelListener() {  
  
            @Override  
            public void onCancel(DialogInterface dialog) {  
                // TODO Auto-generated method stub  
  
            }  
        });  
        //���ÿɵ���İ�ť�����������(Ĭ�������)  
        dialog.setButton(DialogInterface.BUTTON_POSITIVE, "ȷ��",  
                new DialogInterface.OnClickListener() {  
  
                    @Override  
                    public void onClick(DialogInterface dialog, int which) {  
                        // TODO Auto-generated method stub  
  
                    }  
                });  
        dialog.setButton(DialogInterface.BUTTON_NEGATIVE, "ȡ��",  
                new DialogInterface.OnClickListener() {  
  
                    @Override  
                    public void onClick(DialogInterface dialog, int which) {  
                        // TODO Auto-generated method stub  
  
                    }  
                });  
        dialog.setButton(DialogInterface.BUTTON_NEUTRAL, "����",  
                new DialogInterface.OnClickListener() {  
  
                    @Override  
                    public void onClick(DialogInterface dialog, int which) {  
                        // TODO Auto-generated method stub  
  
                    }  
                });  
        dialog.setMessage("����һ��Բ�ν�����");  
        dialog.show();  
        new Thread(new Runnable() {  
  
            @Override  
            public void run() {  
                // TODO Auto-generated method stub  
                try {  
                    Thread.sleep(5000);  
                    // cancel��dismiss�������ʶ���һ���ģ����Ǵ���Ļ��ɾ��Dialog,Ψһ��������  
                    // ����cancel������ص�DialogInterface.OnCancelListener���ע��Ļ�,dismiss��������ص�  
                    dialog.cancel();  
                    // dialog.dismiss();  
                } catch (InterruptedException e) {  
                    // TODO Auto-generated catch block  
                    e.printStackTrace();  
                }  
  
            }  
        }).start();  
		
		
		
		
###�ڶ��ַ�ʽ��ˮƽ������
	// ���������ж�����������������ʽ������Ͳ���ʾ��  
    final ProgressDialog dialog = new ProgressDialog(this);  
    dialog.setProgressStyle(ProgressDialog.STYLE_HORIZONTAL);// ����ˮƽ������  
    dialog.setCancelable(true);// �����Ƿ����ͨ�����Back��ȡ��  
    dialog.setCanceledOnTouchOutside(false);// �����ڵ��Dialog���Ƿ�ȡ��Dialog������  
    dialog.setIcon(R.drawable.ic_launcher);// ������ʾ��title��ͼ�꣬Ĭ����û�е�  
    dialog.setTitle("��ʾ");  
    dialog.setMax(100);  
    dialog.setButton(DialogInterface.BUTTON_POSITIVE, "ȷ��",  
            new DialogInterface.OnClickListener() {  
  
                @Override  
                public void onClick(DialogInterface dialog, int which) {  
                    // TODO Auto-generated method stub  
  
                }  
            });  
    dialog.setButton(DialogInterface.BUTTON_NEGATIVE, "ȡ��",  
            new DialogInterface.OnClickListener() {  
  
                @Override  
                public void onClick(DialogInterface dialog, int which) {  
                    // TODO Auto-generated method stub  
  
                }  
            });  
    dialog.setButton(DialogInterface.BUTTON_NEUTRAL, "����",  
            new DialogInterface.OnClickListener() {  
  
                @Override  
                public void onClick(DialogInterface dialog, int which) {  
                    // TODO Auto-generated method stub  
  
                }  
            });  
    dialog.setMessage("����һ��ˮƽ������");  
    dialog.show();  
    new Thread(new Runnable() {  
  
        @Override  
        public void run() {  
            // TODO Auto-generated method stub  
            int i = 0;  
            while (i < 100) {  
                try {  
                    Thread.sleep(200);  
                    // ���½������Ľ���,���������߳��и��½���������  
                    dialog.incrementProgressBy(1);  
                    // dialog.incrementSecondaryProgressBy(10)//�������������·�ʽ  
                    i++;  
  
                } catch (Exception e) {  
                    // TODO: handle exception  
                }  
            }  
            // �ڽ���������ʱɾ��Dialog  
            dialog.dismiss();  
  
        }  
    }).start();  