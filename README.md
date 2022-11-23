#include <bits/stdc++.h>
#include <windows.h>

using namespace std;
 
struct Student{
	string name, mssv, lop, gioitinh, sdt, noisinh;
	double gpa;
	int ngaysinh, thangsinh, namsinh, ngaymax;
};
 
struct SV{
	Student s;
	SV *next;
};
 
typedef struct SV* sv;
 
//Cap phat dong mot node moi voi du lieu la so nguyen x
sv makeNode(){
	Student s;
	cout << "Nhap thong tin sinh vien: \n";
	cout << "Nhap MSSV: "; cin >> s.mssv;
	cout << "Nhap ten lop hoc: "; cin >> s.lop;
	cout << "Nhap ho & ten: "; cin.ignore();
	getline(cin, s.name);
	cout << "Nhap GPA: "; cin >> s.gpa;
	    while (s.gpa < 0 || s.gpa > 4){
	    	cout << "Co ve sai, moi nhap lai: ";
	    	cin >> s.gpa;}
	cout << "Nhap so dien thoai: "; cin.ignore();
	getline(cin, s.sdt);
        do {
            cout << "Nhap ngay sinh: ";
            cin >> s.ngaysinh;
        } while (s.ngaysinh < 1 || s.ngaysinh> 31);
      
        do {
            cout << "Nhap thang sinh: ";
            cin >> s.thangsinh;
        } while (s.thangsinh < 1 || s.thangsinh > 12);
           
        do {
            cout << "Nhap nam sinh: ";
            cin >> s.namsinh;
        } while (s.namsinh > 2004 );
           
	switch(s.thangsinh){
		  	case 1:
            case 3:
            case 5:
            case 7:
            case 8:
            case 10:
            case 12:
                 s.ngaymax =31; 
              break;
                case 2:
                if ((s.namsinh%4==0 && s.namsinh%100!=0) || (s.namsinh%400==0)){
				
                	if(s.ngaysinh>29){
                		cout<<"Nhap lai ngay sinh: ";
						cin>>s.ngaysinh; 
					}}
                else {
				
                    if(s.ngaysinh>28){
                    	cout<<"Nhap lai ngay sinh: ";
						cin>>s.ngaysinh; 
					}}
                break;
            case 4:
            case 6:
            case 9:
            case 11:
                if(s.ngaysinh>30){
                	cout<<"Nhap lai ngay sinh: ";
						cin>>s.ngaysinh;
				};
                break;
        }
		  cout<<"Noi sinh: ";cin.ignore();
	      getline(cin, s.noisinh);
	      
		  cout<<"Gioi tinh( G or B ): ";
		  cin>>s.gioitinh; 
		  if(s.gioitinh!="G" && s.gioitinh!="B"){
		  	cout<<"Nhap lai gioi tinh : ";
		  	cin>>s.gioitinh;
		} 
	sv tmp = new SV();
	tmp->s = s;
	tmp->next = NULL;
	return tmp;
}
 
//Kiem tra rong
bool empty(sv a){
	return a == NULL;
}
 
int Size(sv a){
	int cnt = 0;
	while(a != NULL){
		++cnt;
		a = a->next; // gan dia chi cua not tiep theo cho node hien tai
		//cho node hien tai nhay sang not tiep theo
	}
	return cnt;
}
 
//them 1 sinh vien vao dau danh sach lien ket
void insertFirst(sv &a){
	sv tmp = makeNode();
	if(a == NULL){
		a = tmp;
	}
	else{
		tmp->next = a;
		a = tmp;
	}
}
 
//Them 1 sinh vien vao cuoi dslk
void insertLast(sv &a){
	sv tmp = makeNode();
	if(a == NULL){
		a = tmp;
	}
	else{
		sv p = a;
		while(p->next != NULL){
			p = p->next;
		}
		p->next = tmp;
	}
}
 
//Them 1 sinh vien vao vi tri bat ky
void insertMiddle(sv &a,int pos){
	int n = Size(a);
	if(pos <= 0 || pos > n + 1){
		cout << "Vi tri them khong hop le !\n"; return;
	}
	if(pos == 1){
		insertFirst(a); return;
	}
	else if(pos == n + 1 ){
		insertLast(a); return;
	}
	sv p = a;
	for(int i = 1; i < pos - 1; i++){
		p = p->next;
	}
	sv tmp = makeNode();
	tmp->next = p->next;
	p->next = tmp;
}
 
//xoa sinh vien o dau
void deleteFirst(sv &a){
	if(a == NULL) return;
	a = a->next;
}
 
//xoa sinh vien o cuoi
void deleteLast(sv &a){
	if(a == NULL) return;
	sv truoc = NULL, sau = a;
	while(sau->next != NULL){
		truoc = sau;
		sau = sau->next;
	}
	if(truoc == NULL){
		a = NULL;
	}
	else{
		truoc->next = NULL;
	}
}
 
//Xoa sinh vien o vi tri bat ky
void deleteMiddle(sv &a, int pos){
	if(pos <=0 || pos > Size(a)) return;
	sv truoc = NULL, sau = a;
	for(int i = 1; i < pos; i++){
		truoc = sau;
		sau = sau->next;
	}
	if(truoc == NULL){
		a = a->next;
	}
	else{
		truoc->next = sau->next;
	}
}
 
void in(Student s){
	cout << "--------------------------------\n";
	cout << "MSSV: " << s.mssv << endl;
	cout << "Lop hoc: " << s.lop << endl;
	cout << "Ho & ten: " << s.name << endl;
    cout << "GPA: " << fixed << setprecision(2) << s.gpa << endl;
	cout << "SDT: " << s.sdt << endl;
	cout << "Ngay/thang/nam sinh: " <<s.ngaysinh<<"/"<<s.thangsinh<<"/"<<s.namsinh<<endl;
	cout << "Noi sinh: " <<s.noisinh<<endl;
    cout << "Gioi tinh: "<<s.gioitinh<<endl; 
	cout << "--------------------------------\n";
}

void inds(sv a){
	cout << "Danh sach sinh vien :\n";
	while(a != NULL){
		in(a->s);
		a = a->next;
	}
	cout << endl;
}
// sap xep cac sinh vien theo gpa
void sxgpa(sv &a){
	for(sv p = a; p->next != NULL; p = p->next){
		sv min = p;
		for(sv q = p->next; q != NULL; q = q->next){
			if(q->s.gpa < min->s.gpa){
				min = q;
			}
		}
		Student tmp = min->s;
		min->s = p->s;
		p->s = tmp;
	}
}
// sap xep cac sinh vien theo id
void sxmssv(sv &a){
	for(sv p = a; p->next != NULL; p = p->next){
		sv min = p;
		for(sv q = p->next; q != NULL; q = q->next){
			if(q->s.mssv < min->s.mssv){
				min = q;
			}
		}
		Student tmp = min->s;
		min->s = p->s;
		p->s = tmp;
	}
}

void tao_mau( int code ) {
    HANDLE color = GetStdHandle(STD_OUTPUT_HANDLE);
    SetConsoleTextAttribute( color , code );
}
 
//Tim kiem trong dslk
//Tim phan tu lon nhat, nho nhat
//Tim kiem sinh vien theo ten, theo ma, id ...
 
 
int main(){
	sv head = NULL;
	while(1){
		tao_mau( 10 );
		cout << " CHUONG TRINH QUAN LY SINH VIEN " << endl;
		tao_mau( 11 );
		cout << "==================== MOI NHAP YEU CAU ====================\n";
		tao_mau( 15 );
		cout << "**||**   1. Them sinh vien vao dau danh sach        **||**\n";
		cout << "**||**   2. Them sinh vien vao cuoi danh sach       **||**\n";
		cout << "**||**   3. Them sinh vien vao vi tri bat ky        **||**\n";
		cout << "**||**   4. Xoa sinh vien o dau danh sach           **||**\n";
		cout << "**||**   5. Xoa sinh vien o cuoi danh sach          **||**\n";
		cout << "**||**   6. Xoa sinh vien o vi tri bat ky           **||**\n";
		cout << "**||**   7. Hien thi danh sach cac sinh vien        **||**\n";
		cout << "**||**   8. Sap xep cac sinh vien theo GPA          **||**\n";
		cout << "**||**   9. Sap xep cac sinh vien theo MSSV           **||**\n";
		tao_mau( 11 );
		cout << "==========================================================\n";
		tao_mau( 23 );
		cout << "Moi nhap lua chon: ";
		tao_mau( 15 );
		int lc; cin >> lc;
		if(lc == 1){
			insertFirst(head);
		}
		else if(lc == 2){
			insertLast(head);
		}
		else if(lc == 3){
			int pos; cout << "Nhap vi tri can them :"; cin >> pos;
			insertMiddle(head, pos);
		}
		else if(lc == 4){
			deleteFirst(head);
		}
		else if(lc == 5){
			deleteLast(head);
		}
		else if(lc == 6){
			int pos; cout << "Nhap vi tri can xoa:"; cin >> pos;
			deleteMiddle(head, pos);
		}
		else if(lc == 7){
			inds(head);
		}
		else if(lc == 8){
			sxgpa(head);
		}
		else if(lc == 9){
			sxmssv(head);
		}
		else if(lc == 0){
			break;
		}
	}
	return 0;
}
 
