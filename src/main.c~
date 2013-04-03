/* -*- Mode: C; indent-tabs-mode: t; c-basic-offset: 4; tab-width: 4 -*- */
/*
 * main.c
 * Copyright (C) Nehal 2011 <total.sniper@gmail.com>
 * 
 * Sniper_GrubManager is free software: you can redistribute it and/or modify it
 * under the terms of the GNU General Public License as published by the
 * Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 * 
 * Sniper_GrubManager is distributed in the hope that it will be useful, but
 * WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
 * See the GNU General Public License for more details.
 * 
 * You should have received a copy of the GNU General Public License along
 * with this program.  If not, see <http://www.gnu.org/licenses/>.
 */

#include <config.h>
#include <gtk/gtk.h>
#include <stdio.h>
#include <string.h>
#include <glib/gi18n.h>



/* For testing propose use the local (not installed) ui file */
/* #define UI_FILE PACKAGE_DATA_DIR"/sniper_grubmanager/ui/sniper_grubmanager.ui" */
#define UI_FILE "src/sniper_grubmanager.ui"

/* Signal handlers */
/* Note: These may not be declared static because signal autoconnection
 * only works with non-static methods
 */

/* Called when the window is closed */

GtkLabel *label;
GtkLabel *label1;
GtkWidget *dialog,*dialog1,*dialog2;
GtkWidget *window1;

int renameFile,temp;
int F1_VER,F2_VER;
char *file1,*file2,*file3;
char *word2find,*word2repl,*word2find1,*word2repl1;
size_t find_len,find_len1,find_len2;
FILE *fp1,*fp2;

void
destroy (GtkWidget *widget, gpointer data)
{
	gtk_main_quit ();
}

static GtkWidget*
create_window (void)
{
	GtkWidget *window;
	GtkBuilder *builder;
	GError* error = NULL;

	
	/* Load UI from file */
	builder = gtk_builder_new ();
	if (!gtk_builder_add_from_file (builder, UI_FILE, &error))
	{
		g_warning ("Couldn't load builder file: %s", error->message);
		g_error_free (error);
	}

	/* Auto-connect signal handlers */
	gtk_builder_connect_signals (builder, NULL);
	label = GTK_WIDGET (gtk_builder_get_object (builder, "label1"));
	label1 = GTK_WIDGET (gtk_builder_get_object (builder, "label2"));
	window1 = GTK_WIDGET (gtk_builder_get_object (builder, "window"));
	/* Get the window object from the ui file */
	window = GTK_WIDGET (gtk_builder_get_object (builder, "window"));
	g_object_unref (builder);
	
	return window;
}

void
on_filebtn1_select (GtkFileChooser *filechooser, gpointer user_data)
{
	gtk_label_set_text (label,gtk_file_chooser_get_filename (filechooser));
	file1=gtk_file_chooser_get_filename (filechooser);
}

void
on_filebtn2_select (GtkFileChooser *filechooser, gpointer user_data)
{
	gtk_label_set_text (label1,gtk_file_chooser_get_filename (filechooser));
	file2=gtk_file_chooser_get_filename (filechooser);
	file3=g_strconcat(gtk_file_chooser_get_filename (filechooser),"_temp",NULL);
	gtk_label_set_text (label1,file2);
}

void
on_click_exit (GtkFileChooser *filechooser, gpointer user_data)
{
	gtk_main_quit ();
}

void
on_click_about (GtkFileChooser *filechooser, gpointer user_data)
{
		dialog1 = gtk_message_dialog_new (window1,
                                 GTK_DIALOG_DESTROY_WITH_PARENT,
                                 GTK_MESSAGE_QUESTION,
                                 GTK_BUTTONS_CLOSE,
                                 "Application version = 0.1\n\nLicence = Freeware (General Public Licence 2)\n\nDeveloper = Nehal(Sniper)\n\n\nIf you found any bug please kindly report us at total.sniper@gmail.com");
		g_signal_connect_swapped (dialog1,
                             "response",
                             G_CALLBACK (gtk_widget_destroy),
                             dialog1);
		gtk_widget_show_all (dialog1);
}

void
on_click_manage (GtkButton *button, gpointer user_data)
{
	temp=0;
	F1_VER=0;
	F2_VER=0;
	word2find="title";
	word2find1="menuentry";
	find_len1 = strlen(word2find1);
	find_len = strlen(word2find);
	find_len++;
	find_len1++;
	char str[find_len];
	char str1[find_len1];
	fp1 = fopen(file1,"r");

    while(fgets(str,find_len,fp1)!=NULL)
    {
		if(strcmp(str,word2find)==0)
		{
			F1_VER=1;
			break;
		}
    }
	
	fclose(fp1);
	fp1 = fopen(file1,"r");
	
	while(fgets(str1,find_len1,fp1)!=NULL)
    {
		if(strcmp(str1,word2find1)==0)
		{
			F1_VER=2;
			break;
		}
    }
	fclose(fp1);
	
    fp2 = fopen(file2,"r");

    while(fgets(str,find_len,fp2)!=NULL)
    {
		if(strcmp(str,word2find)==0)
		{
			F2_VER=1;
			break;
		}
    }
	fclose(fp2);
	fp2 = fopen(file2,"r");

    while(fgets(str1,find_len1,fp2)!=NULL)
    {
		if(strcmp(str1,word2find1)==0)
		{
			F2_VER=2;
			break;
		}
    }
	fclose(fp2);

	if(F2_VER==1 && F1_VER==1)
	{
		fp1 = fopen(file1,"r");
		fp2 = fopen(file2,"a");

	    while(fgets(str,find_len,fp1)!=NULL)
		{
			if(strcmp(str,word2find)==0)
			{
				fputs(str,fp2);
				while(fgets(str,find_len,fp1)!=NULL)
					fputs(str,fp2);
				break;
			}
		}
		fclose (fp1);
		fclose (fp2);
	}
	else if(F2_VER==2 && F1_VER==2)
	{
		word2find="menuentry";
		find_len1 = strlen(word2find);
		find_len1++;
		char str1[find_len1];
		fp1 = fopen(file1,"r");
		fp2 = fopen(file2,"a");

	    while(fgets(str1,find_len1,fp1)!=NULL)
		{
			if(strcmp(str1,word2find)==0)
			{
				fputs(str1,fp2);
				while(fgets(str1,find_len1,fp1)!=NULL)
					fputs(str,fp2);
				break;
			}
		}
		fclose (fp1);
		fclose (fp2);
	}
	else if(F1_VER==1 && F2_VER==2)
	{
		word2find="title";
		fp1 = fopen(file1,"r");
		fp2 = fopen(file2,"a");
		find_len = strlen(word2find);
		find_len++;

	    while(fgets(str,find_len,fp1)!=NULL)
		{
			if(strcmp(str,word2find)==0)
			{
				fputs(str,fp2);
				while(fgets(str,find_len,fp1)!=NULL)
					fputs(str,fp2);
				break;
			}
		}
		fclose (fp1);
		fclose (fp2);

		word2find1="title"; 
		word2repl1="menuentry";
		find_len1 = strlen(word2find1);
		find_len2 = strlen(word2repl1);
		find_len1++;
		find_len2++;
		char str1[find_len1];

		fp1 = fopen(file2,"r");
		fp2 = fopen(file3,"w");
	
		while(fgets(str1,find_len1,fp1)!=NULL)
		{
			if(strcmp(str1,word2find1)==0)
				fputs(word2repl1,fp2);
			else
				fputs(str1,fp2);
		}
		fclose(fp1);
		fclose(fp2);
	
		fp1 = fopen(file2,"w");
		fp2 = fopen(file3,"r");

		word2find1=" root ("; 
		word2repl1=" set root=(";
		find_len1 = strlen(word2find1);
		find_len2 = strlen(word2repl1);
		find_len1++;
		find_len2++;
		char str2[find_len1];

		while(fgets(str2,find_len1,fp2)!=NULL)
		{
			if(strcmp(str2,word2find1)==0)
				fputs(word2repl1,fp1);
			else
				fputs(str2,fp1);
		}
		fclose(fp1);
		fclose(fp2);

		fp1 = fopen(file2,"r");
		fp2 = fopen(file3,"w");

		word2find1=" kernel"; 
		word2repl1=" linux";
		find_len1 = strlen(word2find1);
		find_len2 = strlen(word2repl1);
		find_len1++;
		find_len2++;
		char str3[find_len1];

		while(fgets(str3,find_len1,fp1)!=NULL)
		{
			if(strcmp(str3,word2find1)==0)
				fputs(word2repl1,fp2);
			else
				fputs(str3,fp2);
		}
		fclose(fp1);
		fclose(fp2);
		renameFile=rename(file3,file2);
	}
	else if(F1_VER==2 && F2_VER==1)
	{
		word2find="menuentry";
		find_len1 = strlen(word2find);
		find_len1++;
		char str1[find_len1];
		
		fp1 = fopen(file1,"r");
		fp2 = fopen(file2,"a");

	    while(fgets(str1,find_len1,fp1)!=NULL)
		{
			if(strcmp(str1,word2find)==0)
			{
				fputs(str1,fp2);
				while(fgets(str1,find_len1,fp1)!=NULL)
					fputs(str1,fp2);
				break;
			}
		}
		fclose (fp1);
		fclose (fp2);
		
		word2find1="menuentry"; 
		word2repl1="title";
		find_len1 = strlen(word2find1);
		find_len2 = strlen(word2repl1);
		find_len1++;
		find_len2++;
		char str5[find_len1];
		
		fp1 = fopen(file2,"r");
		fp2 = fopen(file3,"w");
	
		while(fgets(str5,find_len1,fp1)!=NULL)
		{
			if(strcmp(str5,word2find1)==0)
				fputs(word2repl1,fp2);
			else
				fputs(str5,fp2);
		}
		fclose(fp1);
		fclose(fp2);
		
		fp1 = fopen(file2,"w");
		fp2 = fopen(file3,"r");

		word2find1="\tset root"; 
		word2repl1="\ttmp";
		find_len1 = strlen(word2find1);
		find_len2 = strlen(word2repl1);
		find_len1++;
		find_len2++;
		char str2[find_len1];

		while(fgets(str2,(find_len1),fp2)!=NULL)
		{
			if(strcmp(str2,word2find1)==0)
				fputs(word2repl1,fp1);
			else
				fputs(str2,fp1);
			printf("$%s",str2);
		}
		fclose(fp1);
		fclose(fp2);

		fp1 = fopen(file2,"r");
		fp2 = fopen(file3,"w");

		word2find1="\ttmp="; 
		word2repl1="\troot ";
		find_len1 = strlen(word2find1);
		find_len2 = strlen(word2repl1);
		find_len1++;
		find_len2++;
		char str3[find_len1];

		while(fgets(str3,(find_len1),fp1)!=NULL)
		{
			if(strcmp(str3,word2find1)==0)
				fputs(word2repl1,fp2);
			else
				fputs(str3,fp2);
		}
		
		fclose(fp1);
		fclose(fp2);

		fp1 = fopen(file2,"w");
		fp2 = fopen(file3,"r");

		word2find1="\tlinux"; 
		word2repl1="\tkernel";
		find_len1 = strlen(word2find1);
		find_len2 = strlen(word2repl1);
		find_len1++;
		find_len2++;
		char str4[find_len1];

		while(fgets(str4,find_len1,fp2)!=NULL)
		{
			if(strcmp(str4,word2find1)==0)
				fputs(word2repl1,fp1);
			else
				fputs(str4,fp1);
		}
		fclose(fp1);
		fclose(fp2);
	}
	else
	{
		dialog = gtk_message_dialog_new (window1,
                                 GTK_DIALOG_DESTROY_WITH_PARENT,
                                 GTK_MESSAGE_ERROR,
                                 GTK_BUTTONS_CLOSE,
                                 "Sorry, the file you choosed is not a grub config file.\n\nYour original file has been reverted back\n\n\n\nReport the bug at total.sniper@gmail.com if you are sure about grub file.");
		g_signal_connect_swapped (dialog,
                             "response",
                             G_CALLBACK (gtk_widget_destroy),
                             dialog);
		gtk_widget_show_all (dialog);
		temp=1;
	}
	if(temp==0)
	{
		dialog2 = gtk_message_dialog_new (window1,
                                 GTK_DIALOG_DESTROY_WITH_PARENT,
                                 GTK_MESSAGE_INFO,
                                 GTK_BUTTONS_CLOSE,
                                 "Success!!\n\n\nYour Grub has been Successfully managed please restart the system to check.");
		g_signal_connect_swapped (dialog2,
                             "response",
                             G_CALLBACK (gtk_widget_destroy),
                             dialog2);
		gtk_widget_show_all (dialog2);
	}
}

int
main (int argc, char *argv[])
{
 	GtkWidget *window;


#ifdef ENABLE_NLS
	bindtextdomain (GETTEXT_PACKAGE, PACKAGE_LOCALE_DIR);
	bind_textdomain_codeset (GETTEXT_PACKAGE, "UTF-8");
	textdomain (GETTEXT_PACKAGE);
#endif

	
	gtk_init (&argc, &argv);

	window = create_window ();
	gtk_widget_show (window);

	gtk_main ();
	return 0;
}
