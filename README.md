***************IOFile


package controller;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.util.ArrayList;
import java.util.List;

public class IOFile {
	public static <T> List <T> doc(String file){
		List<T> list = new ArrayList<>();
		try {
			ObjectInputStream o =new ObjectInputStream(new FileInputStream(file));
			list = (List<T>)o.readObject();
			o.close();
		}catch(IOException e) {
			System.out.println(e);
		} catch(ClassNotFoundException e) {
			System.out.println(e);
		}
		return list;	
	}
	public static <T>void viet(String file,List<T> arr){
		try {
			ObjectOutputStream o =new ObjectOutputStream(new FileOutputStream(file));
			o.writeObject(arr);
			o.close();
		}catch(IOException e) {
			System.out.println(e);
		}
	}
}






*****************TrongException

package controller;

public class TrongException extends Exception{

}






*****************SinhVien.java
package model;

import java.io.Serializable;

import controller.TrongException;

public class SinhVien implements Serializable{
	private int ma;
	private String ten;
	private String date;
	private String major;
	private static int sma=10000;

	public SinhVien() {
	}
	
	public SinhVien(String ten, String date, String major) throws TrongException {
		if(ten.isEmpty()) throw new TrongException();
		this.ma =sma++;
		this.ten =ten;
                this.date=date;
		this.major = major;
	}

    public int getMa() {
        return ma;
    }

    public void setMa(int ma) {
        this.ma = ma;
    }

    public String getTen() {
        return ten;
    }

    public void setTen(String ten) {
        this.ten = ten;
    }

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }

    public String getMajor() {
        return major;
    }

    public void setMajor(String major) {
        this.major = major;
    }

    public static int getSma() {
        return sma;
    }

    public static void setSma(int sma) {
        SinhVien.sma = sma;
    }

        
public Object[] toObject() {
	return new Object[] {
			ma,ten, date, major
	};
}
}











****************MonHoc.java

package model;

import java.io.Serializable;

import controller.TrongException;

public class MonHoc implements Serializable{
	private int ma;
	private String ten;
	private String stc;
	private String loaimon;
	private static int sma=100;

	public MonHoc() {
	}
	
	public MonHoc(String ten, String loaimon, String stc) throws TrongException {
		if(ten.isEmpty()) throw new TrongException();
		this.ma =sma++;
		this.ten =ten;
                this.stc=stc;
		this.loaimon = loaimon;
	}

    public int getMa() {
        return ma;
    }

    public void setMa(int ma) {
        this.ma = ma;
    }

    public String getTen() {
        return ten;
    }

    public void setTen(String ten) {
        this.ten = ten;
    }

    public String getStc() {
        return stc;
    }

    public void setStc(String stc) {
        this.stc = stc;
    }

    public String getLoaimon() {
        return loaimon;
    }

    public void setLoaimon(String loaimon) {
        this.loaimon = loaimon;
    }

    public static int getSma() {
        return sma;
    }

    public static void setSma(int sma) {
        MonHoc.sma = sma;
    }

    

        
public Object[] toObject() {
	return new Object[] {
			ma,ten, stc, loaimon
	};
}
}



**************Main.java

package view;

import controller.IOFile;
import controller.TrongException;
import java.io.File;
import java.util.ArrayList;
import java.util.List;
import javax.swing.JOptionPane;
import javax.swing.table.DefaultTableModel;
import model.MonHoc;
import model.SinhVien;



   public class Main extends javax.swing.JFrame {
    DefaultTableModel tmMH,tmSV;
    String fMH, fSV;
    List<MonHoc> listMH;
    List<SinhVien> listSV;

    public Main() {
        initComponents();
        fMH="src\\main\\java\\controller\\MH.dat";
        fSV="src\\main\\java\\controller\\SV.dat";
        String[] mh={"ma","ten mon hoc","so tin chi","loai mon"};
        String[] sv = {"ma ","ho ten","ngay sinh","chuyen nganh"};
        tmMH= new DefaultTableModel(mh,0);
        tmSV= new DefaultTableModel(sv,0);
        jTable1.setModel(tmMH);
        jTable2.setModel(tmSV);
        loadMH();
        loadSV();
        showMH(listMH);
        showSV(listSV);
    }
private void loadMH(){
    File f=new File(fMH);
    if(f.exists()){
        listMH=IOFile.doc(fMH);
    }else
        listMH=new ArrayList<>();
}
private void loadSV(){
    File f=new File(fSV);
    if(f.exists()){
        listSV=IOFile.doc(fSV);
    }else
        listSV=new ArrayList<>();
}
private void showMH(List<MonHoc> list){
    tmMH.setRowCount(0);
    for(MonHoc i:list)
        tmMH.addRow(i.toObject());
}
private void showSV(List<SinhVien> list){
    tmSV.setRowCount(0);
    for(SinhVien i:list)
        tmSV.addRow(i.toObject());
}





**********Thêm

 int n= listMH.size();
            if(n>0)
                MonHoc.setSma(listMH.get(n-1).getMa()+1);
            jTextField1.setText(MonHoc.getSma()+"");
            String ten,stc,loaimon;
            try{
                ten = jTextField2.getText();
                stc = jTextField3.getText();
                loaimon = jComboBox1.getSelectedItem().toString();
                MonHoc monhoc=new MonHoc(ten,stc,loaimon);
                tmMH.addRow(monhoc.toObject());
                listMH.add(monhoc);
            }catch(TrongException e){
                JOptionPane.showMessageDialog(this,"khong de trong");
                return;
            }


**********Xoa


    int r=jTable1.getSelectedRow();
        if(r>=0 && r<jTable1.getRowCount()){
            tmMH.removeRow(r);
            listMH.remove(r);
        }else{
            JOptionPane.showMessageDialog(this," chon sach de xoa");
            return;
        }





*******Luu


IOFile.viet(fMH, listMH);
