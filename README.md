#!/usr/bin/perl
use strict;
use warnings;

# File to store tasks
my $file = "tasks.txt";

# Load existing tasks
my @tasks = load_tasks();

while (1) {
    print "\nTo-Do List Manager\n";
    print "1. Add Task\n2. View Tasks\n3. Complete Task\n4. Delete Task\n5. Exit\n";
    print "Choose an option: ";
    my $choice = <STDIN>;
    chomp($choice);

    if ($choice == 1) {
        add_task();
    } elsif ($choice == 2) {
        view_tasks();
    } elsif ($choice == 3) {
        complete_task();
    } elsif ($choice == 4) {
        delete_task();
    } elsif ($choice == 5) {
        save_tasks();
        print "Goodbye!\n";
        last;
    } else {
        print "Invalid option. Try again.\n";
    }
}

sub load_tasks {
    open my $fh, '<', $file or return ();
    chomp(my @tasks = <$fh>);
    close $fh;
    return @tasks;
}

sub save_tasks {
    open my $fh, '>', $file or die "Cannot open $file: $!";
    print $fh "$_\n" for @tasks;
    close $fh;
}

sub add_task {
    print "Enter new task: ";
    my $task = <STDIN>;
    chomp($task);
    push @tasks, "[ ] $task";
    print "Task added!\n";
}

sub view_tasks {
    if (@tasks) {
        print "\nYour Tasks:\n";
        for my $i (0..$#tasks) {
            print "$i. $tasks[$i]\n";
        }
    } else {
        print "No tasks available.\n";
    }
}

sub complete_task {
    view_tasks();
    print "Enter task number to complete: ";
    my $num = <STDIN>;
    chomp($num);
    if ($num =~ /^\d+$/ && $num >= 0 && $num <= $#tasks) {
        $tasks[$num] =~ s/\[ \]/[x]/;
        print "Task marked as completed!\n";
    } else {
        print "Invalid task number.\n";
    }
}

sub delete_task {
    view_tasks();
    print "Enter task number to delete: ";
    my $num = <STDIN>;
    chomp($num);
    if ($num =~ /^\d+$/ && $num >= 0 && $num <= $#tasks) {
        splice(@tasks, $num, 1);
        print "Task deleted!\n";
    } else {
        print "Invalid task number.\n";
    }
}
